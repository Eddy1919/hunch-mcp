# Hunch MCP

Hunch gives your AI assistant calibrated judgments about text. For each text you send, it returns the probability that the answer to a yes/no question is yes, one option from your own list, a score on your own scale, or several yes/no answers at once. The answers come from Jev, TypeSafe's System One model, which returns numbers instead of generated text, so your assistant can sort, filter and count them.

The server is hosted at `https://hunchsheet.app/mcp` (Streamable HTTP). This repo holds the install files for Claude Code and Gemini CLI. Setup for Claude, ChatGPT, Cursor and VS Code is at [hunchsheet.app/connectors](https://hunchsheet.app/connectors).

## Tools

| Tool | Returns |
| --- | --- |
| `hunch_ask` | The probability of yes, 0 to 1, for each text |
| `hunch_pick` | One option from your list for each text, with its confidence |
| `hunch_score` | A position on your levels (0 to n-1), the most likely level, and its confidence |
| `hunch_multi` | One probability per question for each text, up to 10 questions |
| `hunch_balance` | Credits left on your key |

Good for leads, support tickets, reviews, survey answers, emails and any list of short texts you want judged the same way. It judges text only: no arithmetic, counting or dates. English works best. Put the definition inside the question, for example "Can this person approve a software purchase without asking someone else?" instead of "Is this a decision maker?".

## Get a key

The form at [hunchsheet.app](https://hunchsheet.app/#trial) emails a key with 100 free rows. After that, 5,000 rows cost $29 and credits never expire. One credit per answered text per question. Blank texts and repeats in the same call are free.

## Claude Code

```
/plugin marketplace add Eddy1919/hunch-mcp
/plugin install hunch@hunch-mcp
```

Then run `/mcp`, pick `hunch` and choose **Authenticate**. A hunchsheet.app page opens in your browser: paste your key and click **Connect**. The key never goes into a file, and Claude Code renews the sign-in on its own.

The plugin adds the server, a skill that tells Claude when to use each tool, and three commands for CSV files:

| Command | Writes back, for each row |
| --- | --- |
| `/hunch:hunch-check <file.csv> <column> <yes/no question>` | The probability of yes |
| `/hunch:hunch-tag <file.csv> <column> <label1,label2,...> [question]` | One of your labels, with its confidence |
| `/hunch:hunch-score <file.csv> <column> <question> <level1,...,levelN>` | A level from your scale, with its confidence |

For scripts or CI where no browser is available, add the server with the key in a header instead of installing the plugin:

```bash
claude mcp add --transport http hunch https://hunchsheet.app/mcp --header "Authorization: Bearer hunch_your_key_here"
```

## Gemini CLI

```bash
gemini extensions install https://github.com/Eddy1919/hunch-mcp
```

Gemini CLI asks for your key during install and stores it as a sensitive setting.

## Claude, ChatGPT, Cursor and VS Code

- **Claude (claude.ai and Claude Desktop)**: add a custom connector with the URL `https://hunchsheet.app/mcp`, then sign in with your key when Claude opens Hunch's page.
- **ChatGPT**: in developer mode, create a connector with the same URL and sign in the same way.
- **Cursor and VS Code**: one-click install buttons on [hunchsheet.app/connectors](https://hunchsheet.app/connectors).

## Data

The texts and question in a tool call go to the Hunch server and to TypeSafe to compute the answer, and are not stored. Details at [hunchsheet.app/privacy](https://hunchsheet.app/privacy).

## License

The files in this repo are MIT licensed. The Hunch service has its own [terms](https://hunchsheet.app/terms).
