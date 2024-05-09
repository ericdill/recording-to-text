## Getting Started
1. create your env: `mamba env update -f environment.yml`
2. Write your openai key to a file called `.openai` at the root of this git repo. [Generating an OpenAI API Key docs](https://platform.openai.com/docs/quickstart)
3. set up your assemblyai api key
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
