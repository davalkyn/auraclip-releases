# Troubleshooting

| Problem | Fix |
|---|---|
| **"Windows protected your PC"** when installing | Click **More info → Run anyway**. Auraclip is new, so SmartScreen doesn't know it yet. |
| **The window closed but Auraclip still runs** | That's on purpose: it waits in the tray for Telegram and agents. Tray icon → **Quit** stops it. |
| **Analysis is slow** | **Settings → Performance → Light**, or close games while it runs. A graphics card helps most. |
| **"Clips folder cannot be written"** | Windows' ransomware protection may block the Videos folder. **Settings → General → Change…** and pick another folder. |
| **AI scoring: LM Studio not reachable** | Open LM Studio → **Developer** → set **Status: Running**. Then **Settings → AI scoring → Check both**. |
| **Model too big / PC freezes** | Pick a smaller model (see the table in [guide 3](03-local-ai-and-agents.md#lm-studio)). |
| **An agent doesn't see Auraclip's tools** | Auraclip must be running (tray icon). Check the address in **Settings → AI scoring → Connected tools**, then restart the agent. |
| **Agent on another PC can't connect** | By design, Auraclip only accepts agents on the same PC. |
| **Hermes doesn't answer in Telegram** | In PowerShell: `hermes gateway status`. If it's stopped: `hermes gateway run`. Then press **Reconnect** in Connected tools. |
| **Post failed: not signed in** | **Accounts → Connect** that site again (sign-ins live in Auraclip's own browser window). |
| **Telegram approval cards don't arrive** | Check the bot token and your chat id (**@userinfobot**), and press **Start** in your bot once. |

Still stuck? Email **contact@irenew.co.za** with **Settings → About → System health** in a screenshot.
