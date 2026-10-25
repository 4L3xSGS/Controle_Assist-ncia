# Controle de Público

**Sistema web de registro, acompanhamento e consolidação de público em eventos.**

> **Status:** protótipo visual versionado; backend, armazenamento, autenticação, testes e publicação operacional ainda pendentes. As informações exibidas na demonstração são fictícias.

## Visão geral

O **Controle de Público** é uma aplicação web responsiva destinada à organização da contagem de pessoas presentes em eventos com setores ou blocos de assentos. Seu objetivo é substituir comunicações e consolidações manuais por um processo padronizado: cada **indicador** recebe uma designação, informa a quantidade de presentes em seu bloco e a coordenação acompanha os resultados em um painel central.

O projeto foi dimensionado inicialmente para **até 100 indicadores**, **dois eventos por ano** e **público usual inferior a 6.000 pessoas**, priorizando simplicidade operacional, rastreabilidade, acessibilidade em celulares e uso de serviços sem contratação adicional.

### Problema que resolve

Em eventos com múltiplos blocos, o recebimento de contagens por mensagens ou anotações dispersas pode causar atrasos, duplicidades, perda de informações e dificuldades para identificar blocos pendentes. O aplicativo organiza a coleta em um único fluxo e disponibiliza a consolidação para os responsáveis.

## Perfis de acesso

| Perfil | Responsabilidades |
| --- | --- |
| **Indicador** | Abrir um link individual, conferir nome, congregação e bloco designado, informar a quantidade de presentes e receber confirmação. |
| **Administrador** | Cadastrar e ativar indicadores, atribuir blocos, criar/abrir/encerrar eventos, acompanhar envios e pendências, autorizar correções e exportar resultados. |

O indicador **não edita** nome, congregação, telefone ou bloco; apenas informa a quantidade. O acesso administrativo deverá ser autenticado e autorizado no servidor.

## Funcionalidades previstas

- **Cadastro de indicadores:** ID estável, nome, congregação, telefone, bloco e situação ativo/inativo.
- **Links individuais:** credenciais aleatórias não previsíveis, com possibilidade de revogação.
- **Eventos:** cadastro e controle de abertura e encerramento.
- **Registro de contagem:** envio de número inteiro não negativo, associado automaticamente ao indicador, evento e bloco.
- **Proteção contra duplicidade:** um registro vigente por designação e evento, salvo correção autorizada.
- **Painel administrativo:** total de público, quantidade de indicadores, contagens recebidas, pendentes, detalhamento por bloco e horários.
- **Correções auditáveis:** registro do responsável, justificativa e valores anterior/novo.
- **Exportação Excel:** relatório consolidado em formato `.xlsx`.
- **Interface responsiva:** utilização em navegadores de celular e computador.

**Decisões a confirmar antes da persistência:** se haverá mais de uma rodada de contagem por evento e se um indicador poderá receber mais de um bloco. Essas escolhas definem a chave de unicidade do registro.

## Jornada operacional

1. O administrador cadastra o evento e os indicadores e atribui os blocos.
2. O sistema emite links individuais e o administrador os distribui.
3. O indicador abre seu link e confere a identificação e o bloco.
4. O indicador digita a quantidade e confirma o envio.
5. O servidor valida autorização, evento e quantidade e grava o registro sem duplicidade.
6. O painel administrativo apresenta resultados e pendências.
7. Após conferência, o administrador encerra o evento e exporta o relatório.

## Arquitetura

| Camada | Tecnologia planejada | Responsabilidade |
| --- | --- | --- |
| Interface | HTML, CSS e JavaScript | Telas responsivas e interação |
| Backend | Google Apps Script | Validações, regras de negócio e autorização |
| Persistência | Google Sheets | Cadastros, eventos, contagens e auditoria |
| Hospedagem operacional | Web App do Apps Script | Acesso por navegador |
| Relatórios | Google Sheets / XLSX | Consolidação e exportação |
| Versionamento | GitHub | Código-fonte, histórico e documentação |

A prévia visual estática poderá ser publicada separadamente, **sem acesso a dados reais**, para acompanhamento do desenvolvimento. Uma prévia pública não equivale ao aplicativo operacional.

### Estrutura atual

```text
/
├── README.md      # Visão do produto e orientações
├── index.html     # Protótipo da área do indicador e painel
├── styles.css     # Estilos e responsividade
└── app.js         # Navegação e simulação local
```

### Modelo de dados planejado

- **Indicadores:** identificação, congregação, telefone, bloco, ativo e controle de credencial.
- **Eventos:** ID, nome, data e status.
- **Contagens:** evento, indicador, bloco, quantidade, data/hora e situação do registro.
- **Configuração:** parâmetros do sistema.
- **Auditoria:** histórico de correções e ações sensíveis.

A planilha real ainda precisa ser criada ou identificada e vinculada ao Apps Script.

## Segurança e privacidade

O sistema lidará com dados pessoais de indicadores. São requisitos:

- **Não armazenar** telefones reais, nomes de participantes, tokens, senhas, IDs privados de planilhas ou arquivos de credenciais neste repositório.
- Não confiar em campos de identidade ou bloco enviados pelo navegador: o backend deverá resolvê-los a partir do link autorizado.
- Usar tokens individuais de alta entropia e revogáveis. O link funciona como credencial de posse e **não prova identidade pessoal**.
- Restringir o acesso administrativo e às planilhas, com verificação de autorização no servidor.
- Usar `LockService` e validação antes da gravação para evitar envios duplicados por concorrência.
- Registrar alterações sensíveis e limitar exposição de telefones.
- Definir política de retenção e compartilhamento de dados antes do uso real.

**A visibilidade do repositório deve ser PRIVATE.** A alteração precisa ser verificada nas configurações do GitHub; o README não altera permissões.

## Como visualizar o protótipo atual

1. Clone o repositório ou baixe os arquivos.
2. Abra `index.html` em um navegador.
3. Alterne entre **Área do indicador** e **Painel administrativo**.
4. Experimente a simulação de contagem.

**Importante:** o botão de simulação não grava dados, não envia informações a um servidor e não substitui os testes da futura aplicação.

## Etapas de desenvolvimento

| Marco | Entrega | Situação |
| --- | --- | --- |
| M0 | Requisitos, arquitetura e manuais iniciais | Documentado |
| M1 | Protótipo visual responsivo | Código inicial versionado |
| M2 | Planilha Google Sheets e Apps Script | Pendente |
| M3 | Links individuais, validação e registro real | Pendente |
| M4 | Administração protegida e painel conectado | Pendente |
| M5 | Correções, auditoria e exportação XLSX | Pendente |
| M6 | Testes integrados, implantação e piloto | Pendente |

Nenhum marco pendente deve ser tratado como implementado apenas por estar descrito neste documento.

## Critérios de aceitação

O sistema estará pronto para operação quando: (1) cada indicador autorizado visualizar a designação correta; (2) registros forem persistidos com data e hora; (3) duplicidades e acessos inválidos forem rejeitados; (4) o administrador conseguir visualizar pendências e totais consistentes; (5) correções forem controladas; (6) o XLSX reproduzir os dados consolidados; e (7) os fluxos forem testados em celulares e com acessos simultâneos.

## Documentação oficial

- [Manual Técnico e de Desenvolvimento](https://docs.google.com/document/d/1xDBdVOBy7vG_fr_sRVK63pZydKh9IUWJuRMBmIXIw8w/edit)
- [Manual do Usuário](https://docs.google.com/document/d/1AbfzcC9weC5dJ4RRgyR4XF40UCjmTjDZVTn1KUL4qYk/edit)

Os manuais devem ser atualizados junto com mudanças funcionais relevantes. O GitHub registra o código versionado; os manuais registram especificações, decisões e orientações.

## Custos

A solução foi concebida para operar com recursos gratuitos disponíveis nas contas utilizadas, respeitando cotas e limitações dos provedores. **Não autorizar upgrades, contratações ou recursos pagos sem aprovação expressa.** A disponibilidade gratuita deve ser validada na implantação.

---

**Última revisão do README:** 25/10/2026 · **Projeto em desenvolvimento — não utilizar para contagens oficiais nesta fase.**
