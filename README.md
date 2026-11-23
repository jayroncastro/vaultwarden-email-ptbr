# Vaultwarden Email Templates — Português do Brasil (pt-BR)

> **English:** Complete Brazilian Portuguese (`pt-BR`) translation of all 65 official email templates for [Vaultwarden](https://github.com/dani-garcia/vaultwarden) (`v1.37.5`), preserving 100% of native Handlebars variables, conditionals, and `data-testid` attributes. Ready for `TEMPLATES_FOLDER` deployments.

Tradução completa para **português do Brasil (`pt-BR`)** de todos os templates de e-mail transacionais do **Vaultwarden**, mantendo a marca oficial neutra, o layout original e total compatibilidade com o motor Handlebars do servidor.

## Compatibilidade e Garantia Técnica

| Item | Detalhe |
| :--- | :--- |
| **Versão base do Vaultwarden** | `1.37.5` |
| **Cobertura** | **65 de 65 arquivos** (3 parciais de layout + 31 pares HTML e texto puro) |
| **Integridade Handlebars** | 100% das tags (`{{...}}`, `{{{...}}}`, `{{#if}}`), separadores `<!---------------->` e atributos `data-testid` preservados |
| **Codificação e Entregabilidade** | UTF-8 com acentuação consistente nas versões `.hbs` (texto puro) e `.html.hbs` (HTML), evitando penalidades de filtros antispam (ex.: `PARTS_DIFFER`) |
| **Identidade Visual** | Neutra (mantém o branding padrão do Vaultwarden e suporte nativo a `SMTP_EMBED_IMAGES`) |

## Estrutura do Repositório

```text
vaultwarden-email-ptbr/
├── LICENSE
├── README.md
└── templates/
    └── email/
        ├── email_header.hbs
        ├── email_footer.hbs
        ├── email_footer_text.hbs
        ├── admin_account_recovery.hbs
        ├── admin_account_recovery.html.hbs
        ├── change_email.hbs
        ├── change_email.html.hbs
        └── ... (65 arquivos no total)
```

## Como Usar

O Vaultwarden carrega templates personalizados a partir da variável de ambiente `TEMPLATES_FOLDER` (que deve apontar para o diretório pai da pasta `email/`).

### Opção 1 — Docker Compose com Bind Mount (mais simples)

1. Clone este repositório no host onde o Vaultwarden é executado:
   ```bash
   git clone --branch v1.37.5 [https://github.com/jayroncastro/vaultwarden-email-ptbr.git](https://github.com/jayroncastro/vaultwarden-email-ptbr.git)
   ```
2. Monte a pasta `templates` em modo somente leitura e defina `TEMPLATES_FOLDER` no seu `docker-compose.yml`:
   ```yaml
   services:
     vaultwarden:
       image: vaultwarden/server:1.37.5
       environment:
         TEMPLATES_FOLDER: "/opt/vw-templates"
       volumes:
         - vw-data:/data
         - ./vaultwarden-email-ptbr/templates:/opt/vw-templates:ro
   ```

### Opção 2 — Container Único via `Dockerfile` (ideal para Portainer / GitOps)

Para ambientes gerenciados via Git no Portainer sem dependência de *bind mounts* no host:

```dockerfile
FROM vaultwarden/server:1.37.5
COPY templates /opt/vw-templates
```

No `docker-compose.yml`, substitua `image:` por `build: .` e adicione `TEMPLATES_FOLDER: "/opt/vw-templates"`.

### Opção 3 — Pasta padrão dentro do volume `/data`

Se preferir não configurar `TEMPLATES_FOLDER`, copie a pasta `templates/` diretamente para dentro do volume persistente do Vaultwarden (`/data/templates/email/`) e reinicie o container.

## Validação Pós-Implantação

1. Acesse o painel administrativo em `https://<seu-dominio>/admin/diagnostics`.
2. Confirme que o indicador **`Uses custom templates`** está marcado como **`Yes`**.
3. Em **Settings → SMTP Email Settings → Test SMTP**, envie um e-mail de teste e valide o assunto e o corpo em português.

## Observações Técnicas

- **Fixação de versão recomendada:** Ao utilizar templates customizados, recomenda-se fixar a tag da imagem (ex.: `vaultwarden/server:1.37.5`) em vez de `:latest`, evitando que novas variáveis adicionadas em versões futuras do Vaultwarden fiquem ausentes nos arquivos `.hbs` antes da atualização deste repositório.
- **Variável `{{atype}}` no Acesso de Emergência:** Os valores dessa variável (`view` / `take over`) são injetados diretamente pelo binário Rust do Vaultwarden em inglês. Os templates de acesso de emergência foram redigidos de forma a manter a leitura natural em português preservando a variável intacta.
- **Desenvolvimento/Testes:** O Vaultwarden carrega os templates em memória na inicialização. Para testar edições em tempo real sem reiniciar o container, defina temporariamente `RELOAD_TEMPLATES=true` (não recomendado em produção).

## Glossário Adotado (`pt-BR`)

| Original (EN) | Tradução (`pt-BR`) |
| :--- | :--- |
| Vault / Web Vault | Cofre / Cofre web |
| Master Password | Senha mestra |
| Organization | Organização |
| Two-step login / Two-factor | Verificação em duas etapas |
| Emergency Access | Acesso de emergência |

## Contribuição

Encontrou algum ajuste gramatical, inconsistência de termo ou atualização necessária para uma nova versão do Vaultwarden? Abra uma **Issue** ou envie um **Pull Request** mantendo todas as tags Handlebars e o padrão `pt-BR` do glossário acima.

## Licença

Distribuído sob a licença **GNU General Public License v3.0 (GPL-3.0)**, em conformidade com o projeto oficial [dani-garcia/vaultwarden](https://github.com/dani-garcia/vaultwarden).
