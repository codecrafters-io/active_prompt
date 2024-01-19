# Output Parsers

Output Parsers are used to parse the responses received from an LLM.

Each prompt can choose which output parser to use by using the `output` method:

```ruby
class MyPrompt < ActivePrompt::Prompt
  output :text # This is the default
end
```

## Registering Output Parsers

The `register_output_parser` method is used to register an output parser:

```ruby
ActivePrompt.register_output_parser("text", ActivePrompt::OutputParsers::Text)
```

## Default Output Parsers

By default, we register the following output parsers:

```ruby
ActivePrompt.register_output_parser("text", ActivePrompt::OutputParsers::Text)
ActivePrompt.register_output_parser("json", ActivePrompt::OutputParsers::JSON)
```

## Custom Output Parsers

You can define your own output parsers by subclassing `ActivePrompt::OutputParsers::Base`:

```ruby
class MyOutputParser < ActivePrompt::OutputParsers::Base
  def parse(prompt, response)
    # Parse the output here
  end
end
```
