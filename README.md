# Controle de disponibilidade de baterias

App estático para publicar no GitHub Pages. Registra bloqueios e desbloqueios de 480 painéis, mantém histórico, calcula disponibilidade, mostra uma carta de controle e imprime o relatório geral.

## Publicar no GitHub Pages

1. Copie o conteúdo desta pasta para a raiz do repositório GitHub que publica `Controle-de-baterias-3`.
2. Siga a configuração do Firebase abaixo antes de publicar. O arquivo `firebase-config.js` está com valores de exemplo e precisa receber as informações do seu projeto.
3. Envie os arquivos ao GitHub. O Pages deve servir o `index.html` pela raiz.

## Configurar o Firebase (Realtime Database)

O Firebase guarda o estado atual e o histórico de bloqueios/desbloqueios. As regras limitam leitura e gravação ao UID de um operador autenticado.

1. No Firebase Console, crie/abra um projeto e habilite **Authentication → Sign-in method → Email/Password**.
2. Crie um usuário operador em **Authentication → Users → Add user**. Anote o UID desse usuário.
3. Crie um **Realtime Database**. Copie a URL exibida, como `https://seu-projeto-default-rtdb.firebaseio.com` ou `https://seu-projeto-default-rtdb.REGIAO.firebasedatabase.app`.
4. Nas configurações do app Web Firebase, copie o **Web API Key** e preencha `apiKey` e `databaseURL` em `firebase-config.js`. O controle de acesso é feito pelas regras e pela autenticação.
5. Em `database.rules.json`, substitua `COLE_AQUI_O_UID_DO_OPERADOR` pelo UID do operador. Não deixe o texto de exemplo.
6. Publique as regras. Pelo Firebase CLI, execute na pasta do projeto: `firebase deploy --only database` (depois de instalar/autenticar o CLI e associar o projeto). Alternativamente, configure as mesmas regras em **Realtime Database → Rules** no Console.
7. Publique os arquivos no GitHub Pages. Abra o site e entre com o e-mail e a senha do operador criado no passo 2.

**Operadores adicionais:** para permitir outros usuários, adicione os UIDs explicitamente nas duas expressões das regras, por exemplo `auth.uid === 'UID_1' || auth.uid === 'UID_2'`. Não substitua por `auth != null`, pois isso permitiria que qualquer usuário autenticado lesse e alterasse os registros.

## Arquivos

- `index.html`: interface e mapa dos painéis.
- `enhancements.js`: histórico, estatísticas, carta de controle, relatório e sincronização Firebase.
- `firebase-config.js`: configuração pública do app Web Firebase; preencher antes de publicar.
- `database.rules.json`: regras do Realtime Database, restritas a UID(s) autorizados.
- `firebase.json`: configuração opcional para publicar regras/hosting pelo Firebase CLI.

## Indicadores

- Disponibilidade por bateria = (40 − painéis bloqueados) ÷ 40 × 100.
- Indisponibilidade por bateria = painéis bloqueados ÷ 40 × 100.
- Disponibilidade geral = (480 − painéis bloqueados) ÷ 480 × 100.
- A carta exibe a disponibilidade média diária das 12 baterias. Limites de controle = média da série ± 3 desvios padrão, limitados a 0% e 100%.
- O histórico começa a registrar as movimentações após esta versão. Registros já existentes podem mostrar a data inicial do bloqueio, mas datas antigas de desbloqueio não são recuperáveis.

Sem configurar e autenticar o Firebase, o app mantém o funcionamento local no navegador. Para sincronização entre dispositivos e operadores autorizados, configure o Firebase e entre na conta. Use **Desbloquear** para que a saída apareça no histórico; “Limpar bateria selecionada” remove todos os painéis da bateria.

