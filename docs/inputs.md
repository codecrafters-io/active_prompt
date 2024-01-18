# Inputs

Inputs are values that are passed into a prompt when it is evaluated. They are defined using the `input` method:

```ruby
class MyPrompt < ActivePrompt::Prompt
  input :user
end
```

Inputs can be accessed using the `inputs` method inside blocks like `user_message`, `provider`, `model` etc.:

```ruby
class MyPrompt < ActivePrompt::Prompt
  input :user

  model do
    inputs.user.paid? ? "gpt-4" : "gpt-3"
  end

  user_message
    <<~PROMPT
      Hello, #{inputs.user.name}!
    PROMPT
  end
end
```

## Optional Inputs

By default, inputs are mandatory. If you want to make an input optional, you can pass `optional: true` to the `input` method:

```ruby
class MyPrompt < ActivePrompt::Prompt
  input :user
  input :last_message, optional: true
end

MyPrompt.evaluate!(user: User.first) # => does not raise ActivePrompt::MissingInputError even though last_message is not provided
```

Presence of an input is checked by looking whether the key is present in the inputs hash. `nil` is a valid value for an input.

## Validations

ActivePrompt doesn't provide any built-in functionality to validate inputs aside from the checking for non-optional inputs.

If you need to add additional validations, we recommend adding a `validate_inputs!` method:

```ruby
class MyPrompt < ActivePrompt::Prompt
  input :user

  def validate_inputs!
    raise "User is not paid" unless inputs.user.paid?
  end
end

prompt = MyPrompt.new(user: User.first)
prompt.validate_inputs! # => raises "User is not paid"
```
