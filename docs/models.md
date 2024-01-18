### Models

Models are used to [Providers](./providers.md) when evaluating prompts. The syntax to use them is similar to providers.

You can specify a model directly:

```ruby
class MyPrompt < ActivePrompt::Prompt
  model "gpt-4"
end
```

Or specify a block that returns the model name:

```ruby
class MyPrompt < ActivePrompt::Prompt
  model do
    ENV["IS_STAGING"] == "true" ? "gpt-3" : "gpt-4"
  end
end
```

The block has access to the prompt's inputs, which can be used to dynamically select a model.

```ruby
class MyPrompt < ActivePrompt::Prompt
  input :user

  model do
    inputs.user.paid? ? "gpt-4" : "gpt-3"
  end
end
```
