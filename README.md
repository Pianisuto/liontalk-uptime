# LionTalk — monitor de disponibilidade

Confere, a cada 15 minutos e de fora da infraestrutura do LionTalk, se
`https://liontalk.lionslab.dev` responde:

- `GET /` deve responder 200;
- `POST /v1/auth/nonce` deve responder 200 (a API e o banco estão de pé).

Cada verificação tenta 3 vezes, com 20 s entre as tentativas. Se falhar, a
execução fica vermelha e o GitHub avisa por e-mail. O app continua falando
sem internet: uma queda afeta só login e sincronização.

## Por que é público

Repositórios públicos não gastam minutos do GitHub Actions. O monitor só usa
endereços que já são públicos e não precisa de nenhum segredo.

## Segurança

- Nenhum segredo, token próprio ou credencial.
- Nenhuma ação de terceiros (`uses:`); só `curl` e `gh` do próprio runner.
- Sem gatilho por pull request ou issue: forks não executam nada aqui.
- Token padrão sem permissões; só o job semanal pode gravar
  `atividade.txt`, e só quando o repositório está parado há 45 dias (o GitHub
  desliga agendamentos de repositórios públicos sem atividade por 60 dias).
- O código de login criado pelo teste expira sozinho e é apagado pelo servidor.

Para rodar na hora: aba **Actions → Uptime → Run workflow**.
