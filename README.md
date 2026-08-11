# ABRS — Aliança Brasileira de Sim Racing

Plataforma web para reunir pilotos, ligas, rankings e campeonatos brasileiros de Sim Racing, com foco inicial em F1 24, F1 25 e F1 26.

## Estado atual

O projeto está configurado em **modo local de testes**. Os dados são armazenados no `localStorage` do navegador por meio de uma única camada (`storage.js`). O Firebase está preparado, mas desativado em `config.js` até a integração ser finalizada.

### Contas de teste

| Cargo | Usuário | Senha |
|---|---|---|
| Administrador | `Admin` | `123` |
| Piloto | `Piloto` | `123` |
| Dono de liga | `Liga` | `123` |

O login não diferencia maiúsculas de minúsculas.

## Funcionalidades locais verificáveis

- Login e criação de conta local.
- Cargos: piloto, piloto registrado, dono de liga e administrador.
- Cadastro de piloto oficial por categoria.
- Ranking local de pilotos.
- Perfil com foto e nome de exibição.
- Carteira oficial pública com QR Code, estatísticas e impressão em PDF.
- Cadastro de liga com status pendente.
- Aprovação ou rejeição de liga pelo painel administrativo.
- Alteração automática do cargo para dono de liga após aprovação.
- Lista pública somente com ligas aprovadas.
- Solicitação para entrar em uma liga.
- Cronograma com etapas fixas e edição exclusiva das datas.
- Página completa do Campeonato Brasileiro de Ligas.
- Notícias locais de demonstração.
- Migração automática das antigas chaves de `localStorage`.

## Estrutura principal

```text
ABRS/
├── index.html
├── ranking.html
├── ligas.html
├── liga-perfil.html
├── campeonatos.html
├── campeonato-ligas.html
├── noticias.html
├── perfil.html
├── carteira.html
├── admin.html
├── config.js
├── storage.js
├── firebase.js
├── auth.js
├── app.js
├── ranking.js
├── leagues.js
├── liga-perfil.js
├── championships.js
├── profile.js
├── carteira.js
├── admin.js
├── style.css
```

## Como executar

Use a extensão **Live Server** do VS Code ou um servidor local. Não é recomendado abrir o HTML diretamente pelo Explorador de Arquivos.

```bash
python -m http.server 8000
```

Depois acesse `http://localhost:8000`.

## Configuração

O arquivo `config.js` controla o modo da aplicação:

```javascript
window.ABRS_CONFIG = {
    firebaseEnabled: false,
    localTestMode: true
};
```

Durante os testes, mantenha `firebaseEnabled: false`. Antes da publicação, desative as contas locais e conclua a autenticação Firebase.

## Armazenamento local unificado

As únicas chaves atuais são:

```text
abrs_users
abrs_leagues
abrs_cron
abrs_session
abrs_news
abrs_storage_version
```

`storage.js` migra dados das versões antigas sem executar `localStorage.clear()`.

## Fluxo recomendado de teste

1. Entre como `Piloto / 123`.
2. Registre-se como piloto na página de ranking.
3. Acesse seu perfil e sua carteira.
4. Envie uma liga pela página de campeonatos.
5. Saia e entre como `Admin / 123`.
6. Aprove a liga no painel administrativo.
7. Confira a liga na página pública.
8. Edite uma data do cronograma e confira a mudança nas páginas de campeonatos.

## Firebase

A ponte Firebase está em `firebase.js` e usa a versão compatível do SDK 10.7.0. Ao ativar o Firebase:

1. Habilite Google em Authentication.
2. Crie o Firestore.
3. Revise e publique `firebase-rules.txt`.
4. Defina `firebaseEnabled: true`.
5. Teste login, criação de perfil e permissões em ambiente separado.

O fluxo completo de vinculação **Google + senha própria da ABRS** ainda precisa de uma tela dedicada para criação da senha e recuperação de conta. Não publique o modo local como sistema de segurança real.

## Segurança

- Senhas do modo local ficam no navegador e servem somente para desenvolvimento.
- Cargos no `localStorage` podem ser alterados manualmente pelo usuário.
- O painel local não substitui regras do Firestore.
- Nunca use contas `Admin/123`, `Piloto/123` e `Liga/123` em produção.
- Valide pagamentos, inscrições e premiações por meio de regulamento e backend confiável.
