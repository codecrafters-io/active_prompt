# Outputs

ActivePrompt supports parsing responses into various formats. Out of the box,
ActivePrompt supports text (the default) and JSON formats. You can define your own
[output parsers](./output_parsers.md) as well.

The output format is defined in a prompt class using the `output` method:

```ruby
class MyPrompt < ActivePrompt::Prompt
  output :text # This is the default
end
```

### JSON Output

To parse the output as JSON, use `output :json`

```ruby
class MyPrompt < ActivePrompt::Prompt
  output :json

  system_message do
    <<~PROMPT
      Only respond with JSON. Start your response with a `{` and end it with a `}`.
    PROMPT
  end
end
```

Note that you still need to add instructions in the system_message to get the LLM to output JSON.

If the answer isn't valid JSON, ActivePrompt will raise an error.

### Future Work

- Self-correcting JSON parser (use LLM to fix invalid JSON)
- Struct parser (parse into a custom struct)
