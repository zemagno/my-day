# My Day — HTML5 / PWA (iPhone)

Aplicação estática HTML5, CSS e JavaScript, sem build, sem React e sem backend próprio. Compatível com o schema `supabase_watch_v2.sql` e os inserts `supabase_mock_iphone_seed.sql`.

## Rodar localmente

```bash
cd my-day-app
python3 -m http.server 8080
```

Abra http://localhost:8080 no computador. Para abrir no iPhone, publique a pasta em um hosting HTTPS (Cloudflare Pages, Netlify, Vercel ou equivalente). Não use `file://` para testar a PWA.

## Teste sem banco

Por padrão inicia em **modo demonstração**, com 4 períodos e 17 atividades. As alterações são armazenadas no `localStorage` do navegador. O modo demonstração **não sincroniza** dispositivos e não grava no Supabase.

## Conectar Supabase

1. Crie as tabelas com `supabase_watch_v2.sql` e aplique o patch de ícones, se necessário.
2. Crie um usuário em Supabase Authentication e, se desejar, execute o seed com o UUID desse usuário.
3. No app: **Configurações → Configurar Supabase**.
4. Informe **Project URL** (`https://<project>.supabase.co`) e **Publishable Key** (`sb_publishable_...` ou chave anon legada).
5. Entre com e-mail/senha do usuário Supabase Auth.

**Não use a connection string PostgreSQL, senha do banco, secret key ou service_role key no frontend.** O acesso é via Supabase REST API, protegido por JWT e RLS.

A sessão fica em `sessionStorage` (é necessário login novamente após encerrar a sessão do navegador). A URL e chave pública ficam em `localStorage`.

## Funcionalidades V1

- Página inicial com períodos e contadores de atividades obrigatórias concluídas.
- Lista de atividades obrigatórias e opcionais; tela de detalhe.
- Marcar/desmarcar conclusão de hoje, com registro por data local.
- Histórico diário com navegação por datas.
- Criar, editar e excluir atividades; editar nome, cor e ícone do período.
- Layout responsivo com suporte a modo escuro e instalação PWA (em HTTPS).

## Limitações deliberadas

- Sem edição offline sincronizada; é necessária conexão para operações no Supabase.
- Histórico carrega no máximo 5.000 registros por sessão. Para uso prolongado, implementar paginação por intervalo de datas.
- Os ícones são mapeados para emojis no HTML5; os nomes SF Symbols do banco são preservados para uma futura versão SwiftUI.
- O app não inclui cadastro de novos usuários, recuperação de senha, push notifications ou tela administrativa de reordenação.
- A data local é calculada no fuso horário configurado no iPhone; viagens entre fusos podem mudar a data de conclusão utilizada.
- O projeto Supabase precisa ter login por e-mail/senha habilitado e as políticas RLS do schema V2 aplicadas.

## Arquivos

- `index.html` — entrada da aplicação
- `styles.css` — interface responsiva
- `app.js` — navegação, modo demonstração, Auth e Data API
- `manifest.webmanifest` / `icon.svg` — instalação na tela inicial

## Interação rápida (V1.1)

Na lista de atividades, um toque na linha marca ou desmarca imediatamente a conclusão do dia, sem abrir a tela de detalhes ou pedir confirmação. O estado é salvo no modo demonstração ou no Supabase configurado.
