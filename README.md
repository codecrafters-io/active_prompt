# ActivePrompt

Library to manage LLM prompts. Supports:

- Defining prompts using a lightweight DSL
- Evaluating prompts using multiple providers (OpenAI, Replicate, Azure OpenAI etc.)
- Decoding responses as text, JSON or custom structured data

## Installation

Add this line to your application's Gemfile:

```ruby
gem "active_prompt"
```

And then execute:

```bash
bundle install
```

(or install using RubyGems directly using `gem install active_prompt`)

## Usage

Here's a simple example of a prompt that asks the user a question and then answers it:

```ruby
class AnswerQuestionPrompt < ActivePrompt::Prompt
  provider "openai"
  model "gpt4"

  input :question

  system_message do
    <<~PROMPT
      The user will ask you a question. Your job is to answer truthfully. If you don't know the answer, you can say "I don't know".
    PROMPT
  end

  user_message do
    <<~PROMPT
      I have a question: #{question}?
    PROMPT
  end
end
```

To evaluate a prompt, call `#evaluate!` on the prompt class:

```ruby
AnswerQuestionPrompt.evaluate!(question: "What color is the sky?") # => "The sky is blue."
```

## Advanced Usage

### Providers

Providers are used to evaluate prompts. ActivePrompt comes with built-in providers for OpenAI & Azure OpenAI.

The `register_provider` method is used to register a provider:

```ruby
ActivePrompt.register_provider("openai_staging", ActivePrompt::Providers::OpenAI, api_key: ENV["OPENAI_STAGING_API_KEY"])
```

You can write your own provider by implementing the methods in [`ActivePrompt::Providers::Base`](todo).

The `provider` method is used to specify which provider to use when evaluating the prompt.

```ruby
class MyPrompt < ActivePrompt::Prompt
  provider "openai_staging"
end
```

The `provider` method also accepts a block, which can be used to configure the provider on the fly:

```ruby
class MyPrompt < ActivePrompt::Prompt
  # Either return the provider key to use a registered provider:
  provider do
    ENV["IS_STAGING"] == "true" ? "openai_staging" : "openai"
  end

  # or return a new provider instance:
  provider do
    ActivePrompt::Providers::OpenAI.new(api_key: ENV["OPENAI_API_KEY"])
  end
end
```

### Models

Models are passed to providers when evaluating prompts. The syntax to use them is similar to providers.

```ruby
class MyPrompt < ActivePrompt::Prompt
  # Either specify the model name directly
  model "gpt-4"

  # Or specify a block that returns the model name
  model do
    ENV["IS_STAGING"] == "true" ? "gpt-3" : "gpt-4"
  end

  # The block can also access prompt inputs
  input :user

  model do |inputs|
    inputs.user.paid? ? "gpt-4" : "gpt-3"
  end
end
```
