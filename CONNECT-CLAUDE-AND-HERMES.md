# Connect Claude and Hermes Agent

Nexus Gateway finds AI models on your computer by itself (LM Studio, Ollama, llama.cpp, vLLM). **Claude** and **Hermes Agent** are not found automatically: you connect each one once, in the gateway's setup page. Neither needs any coding.

| | What you need | Time |
|---|---|---|
| **Claude** | An Anthropic API key, with credit on the account | About 5 minutes |
| **Hermes Agent** | Hermes installed, and three lines added to its settings file | About 10 minutes |

Before you start, finish the normal setup in the [README](README.md#get-started), so the app is paired with your computer.

**Opening the setup page:** on the computer running Nexus Gateway, open `http://127.0.0.1:8790` in a browser. It's only reachable from that computer.

---

## Claude

Claude runs in Anthropic's cloud, so it works even when your computer has no model of its own. It is billed by Anthropic, per use, to your own account.

### 1. Make an API key

1. Sign in at [console.anthropic.com](https://console.anthropic.com).
2. Open **Billing** and add some credit. A few dollars is plenty to try it.
3. Open **API keys** › **Create key**. Name it something like "Nexus Hub".
4. Copy the key. It starts with `sk-ant-`, and the console shows it only once.

Treat the key like a password. Anyone who has it can spend your credit.

### 2. Add it to the gateway

1. Open the setup page (`http://127.0.0.1:8790`).
2. Under **Online services and agent servers**, click **Add a connection**.
3. **Kind:** choose **Anthropic (Claude)**. The address fills itself in.
4. **Key:** paste your key.
5. Click **Check and save**. When it works, the connection shows as online, with the Claude models your key can use.

The key is stored only on this computer, never on your iPhone.

### 3. Make a Claude agent in the app

1. In Nexus Hub, open **Agents** › **New agent**.
2. Pick a Claude model from the list, for example Claude Sonnet.
3. Give it a name and, if you like, instructions and a voice. Save.

Each agent uses one Claude model. If you want two, for example a fast one and a deeper one, make one agent for each.

### If it doesn't work

- **"The key was refused":** the key was copied incompletely, or was deleted in the console. Make a new one.
- **"Out of credit":** add credit under Billing in the Anthropic Console.
- **Your bill:** see console.anthropic.com › Usage. Longer conversations cost more, because each message sends the conversation so far.

---

## Hermes Agent

[Hermes Agent](https://github.com/NousResearch/hermes-agent) is an AI agent with its own memory, skills and tools (terminal, files, web). Nexus Hub talks to it through Hermes's built-in **API server**, which is off until you turn it on. Hermes can be on the same computer as Nexus Gateway, or on another computer in your Tailscale network.

### 1. Turn on Hermes's API server

1. Find Hermes's settings file, named `.env`:
   - **Windows:** `%LOCALAPPDATA%\hermes\.env`. Paste that into File Explorer's address bar to open the folder.
   - **Mac and Linux:** `~/.hermes/.env`
2. Open it in a text editor such as Notepad, and add these three lines at the end:

   ```
   API_SERVER_ENABLED=true
   API_SERVER_KEY=make-up-a-long-random-password-here
   API_SERVER_HOST=127.0.0.1
   ```

   - **`API_SERVER_KEY`**: make up a long random password, 30 characters or more. You'll paste the same text into the gateway later. Keep it private.
   - **`API_SERVER_HOST`**:
     - Hermes on the **same computer** as Nexus Gateway: leave it as `127.0.0.1`.
     - Hermes on **another computer**: put that computer's Tailscale address, the `100.x.y.z` number shown in the Tailscale app. This lets only your own Tailscale devices reach Hermes, not everyone on the same Wi-Fi.
3. Save the file, making sure the last line ends with a line break, then restart Hermes's gateway so it reads the new settings.

Hermes's API server listens on port **8642**.

### 2. Add Hermes to Nexus Gateway

1. Open the setup page (`http://127.0.0.1:8790`) on the computer running Nexus Gateway.
2. Under **Online services and agent servers**, click **Add a connection**.
3. **Kind:** choose **Hermes Agent**.
4. **Address:**
   - Same computer: `http://127.0.0.1:8642/v1`
   - Another computer: `http://100.x.y.z:8642/v1`, using that computer's Tailscale address.
5. **Key:** paste the `API_SERVER_KEY` you made up.
6. Click **Check and save**. When it works, Hermes shows as online.

### 3. Make a Hermes agent in the app

1. In Nexus Hub, open **Agents** › **New agent**.
2. Pick Hermes's model from the list (`hermes-agent`, or your Hermes profile's name).
3. **Leave the instructions empty.** Hermes brings its own persona, memory and skills.

### Approvals

When Hermes wants to do something risky, such as run a command, it asks first. The question appears in the chat on your iPhone. Reply **approve** or **deny**.

### If it doesn't work

- **"No answer":**
  - Check that Hermes is running and that you restarted it after editing `.env`.
  - Check that the address and port (8642) are right.
  - For another computer, check that Tailscale is on, on both computers.
  - If Windows asks whether to allow Hermes on networks, allow it.
- **"The key was refused":** the key in the gateway doesn't match `API_SERVER_KEY` in Hermes's `.env`. Watch for spaces at the start or end.
- **Check from the computer running Nexus Gateway.** Open `http://<address>:8642/v1/models` in a browser. An "unauthorized" message means Hermes is reachable and only the key is missing, which is expected. If the page can't be reached at all, the problem is the network or Hermes itself.

---

## Other services

The same **Add a connection** form also connects OpenAI, OpenRouter, Groq, LM Studio or Ollama on another computer, and any OpenAI-compatible server. Each **Kind** shows a short note on where to get its key.

Stuck? Open an issue in this repository, or email support@swiftserviceapp.com.
