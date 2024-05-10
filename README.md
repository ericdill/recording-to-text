## Getting Started
1. create your env: `mamba env update -f environment.yml`
2. Write your openai key to a file called `.openai` at the root of this git repo. [Generating an OpenAI API Key docs](https://platform.openai.com/docs/quickstart)
3. set up your assemblyai api key and write it to a file called `.assemblyai` at the root of this git repo

## Lessons Learned

1. Tokens are how all of these openai models work. Each model has some number of tokens that it can handle at a given time. There are three kinds of tokens that you must understand:
    1. `completion_tokens` is the total number of `tokens` that the model uses to generate its response to your prompt.
    2. `prompt_tokens` is the total number of `tokens` that you provide to the model so that it can generate the output that you've asked it for
    3. `total_tokens` is the sum of `completion_tokens` and `prompt_tokens`. This number of `total_tokens` must be less than:
        1. the rate limits imposed on your account by openai. Rate limits vary by model and by your account tier. You can see this at [this url](https://platform.openai.com/settings/organization/limits). [published rate limits guide](https://platform.openai.com/docs/guides/rate-limits)
        2. the Context Window for the model that you're using. You can see the values for each of these models [here](https://platform.openai.com/docs/models/gpt-4-turbo-and-gpt-4). At the time of this writing (May 2024), the context window for `gpt-4-turbo` is 128,000 tokens. So your total tokens must be less than that.
        3. The `max_tokens` parameter that is passed to the `chat.completions.create` API call.
        4. Other notes: It seems like the max completion_tokens is 4096. I haven't really tried to figure out how to increase that. But you should be aware of this limit if you're expecting a large response from your API call, since your returned chat completion will be truncated at 4096 tokens. This took me a while to learn

## Notes

* can whisper distinguish between speakers?
    * [seems like no](https://community.openai.com/t/can-whisper-distinguish-two-speakers/291253/3), but you can use assemblyai to do this:
        ```python
        import assemblyai as aai
        # Replace with your API token
        aai.settings.api_key = f"{insert api token}"

        # URL of the file to transcribe
        FILE_URL = "https://github.com/AssemblyAI-Examples/audio-examples/raw/main/20230607_me_canadian_wildfires.mp3"

        # You can also transcribe a local file by passing in a file path
        # FILE_URL = './path/to/file.mp3'

        config = aai.TranscriptionConfig(speaker_labels=True)

        transcriber = aai.Transcriber()
        transcript = transcriber.transcribe(
        FILE_URL,
        config=config
        )

        for utterance in transcript.utterances:
        print(f"Speaker {utterance.speaker}: {utterance.text}")
        ```
