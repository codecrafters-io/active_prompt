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
      I have a question: #{inputs.question}?
    PROMPT
  end
end
```

To evaluate a prompt, call `#evaluate!` on the prompt class:

```ruby
AnswerQuestionPrompt.evaluate!(question: "What color is the sky?") # => "The sky is blue."
```

## Examples

TODO

## Advanced Usage

TODO

## Concepts

- [Providers](./docs/providers.md)
- [Models](./docs/models.md)
- [Inputs](./docs/inputs.md)
- [Results](./docs/results.md)
