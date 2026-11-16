# Terras Raras — Mining Tycoon v4.5

## Arquivos
- `index.html`: interface do jogo.
- `style.css`: visual, animações e responsividade.
- `app.js`: regras, missões, produção, áudio e integração Firebase.
- `sw.js`: cache/offline do aplicativo.
- `manifest.json` e `icon.svg`: instalação como PWA.
- `firestore.rules`: regras mínimas para proteger cada save por UID.

## Configuração obrigatória do Firebase
1. No Firebase Console, abra **Authentication → Sign-in method** e habilite **Anonymous**.
2. Abra **Firestore Database → Rules** e publique o conteúdo de `firestore.rules`.
3. Confirme que o projeto Firebase usado em `app.js` é o projeto correto. A configuração web do Firebase identifica o projeto, mas não substitui as regras de segurança.
4. Publique os arquivos em hospedagem HTTPS (Firebase Hosting, GitHub Pages ou servidor local). Service Worker não funciona corretamente abrindo `index.html` por `file://`.

## Como o salvamento funciona
- O jogo grava localmente no navegador e envia o progresso para `saves/{uid}` no Firestore.
- A autenticação anônima mantém o UID neste navegador enquanto os dados do site não forem apagados.
- **Importante:** conta anônima não é uma conta compartilhada entre dispositivos. Para continuar a mesma campanha em outro celular/PC, será necessário adicionar login permanente (Google/e-mail) e vincular a conta anônima.
- Se a nuvem falhar, o jogo mantém a cópia local e mostra o status de erro; não exibe sucesso falso.

## Áudio e interação
Os efeitos sonoros são sintetizados no navegador via Web Audio API; a voz do narrador usa a síntese de voz do dispositivo. O navegador exige um primeiro clique do jogador para liberar áudio.

## Testes recomendados
1. Abra o jogo, faça uma ação e confira o status `Progresso salvo na nuvem`.
2. Atualize a página e confirme que créditos/estoque/missões continuam.
3. No Firestore, confira `saves/{UID exibido no jogo}`.
4. Teste offline: o jogo deve manter o save local e indicar que a nuvem está indisponível.
5. Não use o botão **Novo Jogo** para testar persistência: ele reinicia a campanha.
