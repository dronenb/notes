# VSCodium

## GitHub Copilot

```bash
export VSCODE_GALLERY_SERVICE_URL="https://marketplace.visualstudio.com/_apis/public/gallery"
codium --install-extension GitHub.copilot
codium --install-extension GitHub.copilot-chat
yq -i '.extensionEnabledApiProposals."GitHub.copilot" = ([.extensionEnabledApiProposals."GitHub.copilot", ["inlineCompletions","inlineCompletionsNew","inlineCompletionsAdditions","textDocumentNotebook","interactive","terminalDataWriteEvent"]] | flatten | unique)' /Applications/VSCodium.app/Contents/Resources/app/product.json
```

refs:

- <https://gist.github.com/anxkhn/9ae7b2248999168b73f303dec5851460>
- <https://github.com/VSCodium/vscodium/discussions/1487>
