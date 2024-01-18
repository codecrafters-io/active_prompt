# Providers

Providers are used to evaluate prompts. ActivePrompt comes with built-in providers for OpenAI & Azure OpenAI, but you can write
your own provider.

## Registering Providers

The `register_provider` method is used to register a provider:

```ruby
ActivePrompt.register_provider("openai_staging", ActivePrompt::Providers::OpenAI, api_key: ENV["OPENAI_STAGING_API_KEY"])
```

## Default Providers

By default, we register the following providers:

```ruby
ActivePrompt.register_provider("openai", ActivePrompt::Providers::OpenAI, api_key: ENV["OPENAI_API_KEY"])
```

## Using Providers

The `provider` method is used to specify which provider to use when evaluating a prompt.

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

The `provider` method's block also receives `inputs` as an argument:

```ruby
class MyPrompt < ActivePrompt::Prompt
  input :user

  provider do |inputs|
    if inputs.user.paid?
      "openai"
    else
      "openai_staging"
    end
  end
end
```

## Custom Providers

You can write your own provider by implementing the methods in [`ActivePrompt::Providers::Base`](todo).

```ruby
class CustomProvider < ActivePrompt::Providers::Base
  def initialize(api_key:)
    @api_key = api_key
  end

  def evaluate(prompt)
    # ... implement your logic here
  end
end
```
