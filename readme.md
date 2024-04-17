# NLP Project
This is a fine-tuning pipeline for huggingface LLMs using market data and chat messages to generate a synthetic datasets for sentiment analysis classification.

## Setup
Extract the 2 zip files at the top of the repository. The following folder structure should be present.
```
├── final.ipynb
├── readme.md
├── discord_data
   ├── *.csv
├── trade_data
   ├── SOLUSD.csv
```

## Configuring
The config is located inside of the notebook and look like the following.
```
BATCH_SIZE = 64
EPOCHS = 1
LEARNING_RATE = 2e-05

MIN_MESSSAGE_LEN = 16
MAX_MESSSAGE_LEN = 64
TIME_WINDOW = 60 * 60 * 24 * 7
DELTA_BUFFER = 0.001

TRADE_DATA_PATH = "trade_data"
MESSAGE_DATA_PATH = "discord_data"

SIGNAL_PROCESS_FN = vwap_signals
MESSAGE_PROCESS_FN = noop
```

- `TIME_WINDOW`: Time window to resample to in seconds.
- `SIGNAL_PROCESS_FN`: Function to run over the trade data to produce signal data.
- `MESSAGE_PROCESS_FN`: Function to process messages.

## Saving and Loading the model
At the end of the notebook there are 2 cells that can be used to save and load the trained model from disk. 

## Evaluation
Evaluation metrics are lcoated at the end of the notebook and can be used to asses the accuracy of the model. Note that LAP is most likely not a useful metric in most scenerios.

## Collecting Data
The chat message data was collected via an auxilary tool [DiscordChatExporter](https://github.com/Tyrrrz/DiscordChatExporter).

Following the setup guide and issuing the following command with a valid token will output the chat message data in the correct format. Take note of the UTC flag as all timestamps are expected to be in UTC. 
```
discord-chat-exporter-cli exportall -t <YOUR_TOKEN> --utc --fuck-russia -f Csv
```

The russia flag skips additional messages output by the CLI.
