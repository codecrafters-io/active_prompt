# Responses

ActivePrompt supports two types of responses: `ActivePrompt::PromptResponse` & `ActivePrompt::PromptResponseStream`.

## PromptResponse

`ActivePrompt::PromptResponse` is the default response type. It is returned by `ActivePrompt::Prompt.evaluate`.

Results are a lower-level concept than "Responses".
