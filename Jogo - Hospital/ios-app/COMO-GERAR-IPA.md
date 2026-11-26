# Projeto iOS — Super Enfermagem (iPhone / iPad)

Por restrição da Apple, arquivos `.ipa` assinados para iPhone só podem ser compilados em **macOS com Xcode** (ou em um serviço de build macOS na nuvem, como GitHub Actions `macos-latest` ou Codemagic).

Esta pasta contém o aplicativo nativo iOS completo, **separado da versão de PC**, com:
- `SuperEnfermagemApp.swift` (`WKWebView` offline em tela cheia, sem abrir o Safari)
- `Resources/index.html` (o jogo embutido offline)
- `Assets.xcassets/AppIcon.appiconset/icon-1024.png` (ícone oficial 1024x1024 do app)
- `Info.plist` e `project.yml` (configuração pronta para Xcode / XcodeGen)

## Como gerar o `.ipa` no Mac:
1. Copie a pasta `ios-app` para o Mac.
2. Instale o XcodeGen (`brew install xcodegen`) e rode `xcodegen generate` dentro de `ios-app`.
3. Abra `SuperEnfermagem.xcodeproj` no Xcode, selecione seu time/conta Apple e clique em **Run** (ou **Product → Archive** para exportar o `.ipa`).
