---
marp: true
theme: default
__class: invert
---

# GitHub Copilot

- GitHub Copilot chat
- Providing context
- Copilot edit mode
- Extension
- Try it out and demo

---

# Copilot chat

- [Copilot cheat sheet](https://code.visualstudio.com/docs/copilot/copilot-vscode-features)
- Chat window
- Global chat
- Inline chat
- Custom prompt
- Suggestions
    - Commit messages
    - Terminal
    - Refactoring
    - Search

---

# Providing context

- `#file`, `#codebase`, `#editor`, `#selection`, `#terminalLastCommand`, `#terminalSelection`



---

# Copilot edit mode


---

# Extensions

Have [nodejs](https://nodejs.org/en/download/) installed and run the following:

```
npx --package yo --package generator-code -- yo code

```

---

# Update extension config

Add to package.json:

```json
    "publisher": "your-name",
```

```json
"extensionDependencies": [
    "github.copilot-chat"
  ],
```

```json
"contributes": {
    "chatParticipants": [
      {
        "id": "chatty",
        "name": "chatty",
        "description": "A chat participant that says hello world."
      }
    ],
}
```

---

# Update extension code

In `src/extension.ts`:

```typescript
export function activate(context: vscode.ExtensionContext) {
    vscode.chat.createChatParticipant(
		"chatty",
		async (request, context, response, token) => {
			let query = request.prompt;
			query = `${query} - Always end every message with some form of passive aggressive 'You're welcome from chatty.`;

			const chatModels = await vscode.lm.selectChatModels({ family: "gpt-4" });
			const messages = [vscode.LanguageModelChatMessage.User(query)];
			const chatRequest = await chatModels[0].sendRequest(
				messages,
				undefined,
				token,
		    );
            for await (const token of chatRequest.text) {
				response.markdown(token);
			}
		});
}
```

---
# Note on private extensions

- Not fully supported yet [Issue #21839](https://github.com/Microsoft/vscode/issues/21839)
- You can [distribute and install from a file](https://code.visualstudio.com/docs/editor/extension-marketplace#_common-questions)


---
# Try it out and demo

- Come up with idea and try it out
- Demo it

Examples:
- Get Copilot to build an interface to a DB table, repo, API etc.
- Build a web page for an API
- Build an extension with context of internal docs or libraries

---

# Links

- [Copilot docs](https://docs.github.com/en/copilot)
- [Video: Demo building a Copilot extension](https://youtu.be/OdW2r3raAHI?si=pL0gkRi0Pv7jQUBs)
- [Video: Copilot best practices](https://youtu.be/2q0BoioYSxQ?si=E5i9xQNiKtaWFcnk)
