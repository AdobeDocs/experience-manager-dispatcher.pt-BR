---
source-git-commit: c41b4026a64f9c90318e12de5397eb4c116056d9
workflow-type: tm+mt
source-wordcount: '740'
ht-degree: 3%
---
# Diretrizes para contribuir com a documentação do Adobe Experience Manager

## Filosofia da documentação

Os usuários do Adobe Experience Manager trabalham em ambientes extremamente competitivos, esforçando-se para criar experiências digitais que as destaquem de seus concorrentes. Portanto, quando o Adobe fornece ferramentas avançadas no AEM, elas são complementadas com documentação precisa e transparente. Ele permite que os clientes usem imediatamente seu investimento na AEM e maximizem seu retorno sobre o investimento.

O objetivo é colocar a documentação do AEM nas mãos de usuários do AEM assim que possível. Portanto, a Adobe prioriza uma documentação precisa e utilizável, e se esforça para atualizá-la e aprimorá-la continuamente.

## Contribuições à documentação

Para melhorar continuamente a documentação do AEM, toda a comunidade de usuários do AEM é bem-vinda para contribuir em sua elaboração. Seja por meio de pull requests ou problemas, as melhorias na documentação podem ser correções, esclarecimentos, expansões e outros exemplos.

## Padrões de documentação

Embora a Adobe receba contribuições para sua documentação, qualquer contribuição para a documentação do AEM em um pull request ou um problema deve estar em conformidade com os padrões de contribuição e de documentação do Adobe.

As contribuições que não atenderem a esses padrões poderão ser rejeitadas.

### Os casos de uso padrão estão documentados no Adobe.

A documentação do AEM abrange casos de uso padrão. Casos de uso que não se enquadrarem no escopo de instalação e de uso padrão do produto não farão parte da documentação do AEM.

### O Adobe geralmente não documenta bugs e suas soluções.

A documentação do AEM abrange casos de uso padrão. Por essa razão, os bugs, seus efeitos e soluções alternativas não são documentados.

As exceções a essa regra aplicam-se às notas de versão, nas quais problemas conhecidos podem ser listados com possíveis soluções aprovadas pelo Gerenciamento de produtos do.

### As contribuições à documentação não se destinam a responder perguntas técnicas.

Quaisquer ideias para melhorar a documentação do AEM são bem-vindas como contribuições. Entretanto, quaisquer comentários, problemas e pull requests destinam-se somente a *contribuições*. Eles não são para responder suas perguntas sobre como usar o AEM, implementar seu projeto do AEM ou resolver problemas técnicos.

Você pode relatar qualquer pergunta sobre o uso do AEM ou erros técnicos. Use o processo normal de suporte por meio do [Portal de suporte da Experience Cloud Enterprise](https://experienceleague.adobe.com/pt-br?support-solution=General#support) ou discutido na [comunidade Experience Manager](https://experienceleaguecommunities.adobe.com/t5/adobe-experience-manager/ct-p/adobe-experience-manager-community?profile.language=pt).

***As contribuições à documentação da AEM não substituem o Atendimento ao cliente da Adobe***. Logo, qualquer contribuição que buscar respostas a perguntas relacionadas a suporte será rejeitada.

### As contribuições devem mencionar claramente as páginas de documentação afetadas.

Se você criar um problema para sugerir melhorias na documentação, deverá incluir links para as páginas afetadas. Se você criar um problema usando o link **Editar esta página** em uma página de documentação, o problema será criado automaticamente com um link para a página.

Esse processo não se aplica a pull requests, que já fazem referência às páginas afetadas.

## Diretrizes de documentação

A Adobe solicita que qualquer contribuição à sua documentação siga determinadas diretrizes de estilo.

Seguir essas diretrizes facilita a análise de sua contribuição e, portanto, agiliza a integração à documentação da Adobe.

### Idioma e estilo

#### Idioma

* A documentação do AEM foi criada e mantida em inglês americano.
* Mantenha as frases o mais simples possível.
* Mantenha a linguagem clara e concisa.

Lembre-se de que os leitores da documentação do AEM estão espalhados ao redor do mundo, e não espera-se que sejam falantes nativos ou fluentes em inglês. Evite linguagem coloquial e mantenha-a o mais clara e simples possível.

#### Siga o Manual de estilo da Microsoft®

[O Manual de estilo da Microsoft®](https://learn.microsoft.com/en-us/style-guide/welcome/) é um guia de estilo disponível gratuitamente. Ele se concentra na documentação de softwares, e a documentação da AEM o segue sempre que possível.

### Formatação

| Item | Estilo |
|---|---|
| Elemento ou opção da interface do usuário | **negrito** |
| Nome do arquivo, caminho, entrada do usuário, valores de parâmetro | `monospaced` |
| Código, linha de comando | ```Code Block``` |

### Capturas de tela

As capturas de tela devem ser usadas com critério e somente quando uma descrição textual for insuficiente.

Marcadores ou outras anotações em capturas de tela (como quadros vermelhos, setas ou texto) não devem ser usados. Dessa forma, as capturas de tela são mais fáceis de reutilizar ou replicar em versões localizadas da documentação.

### Referências específicas à versão

Evite referências diretas a uma versão específica em todo o conteúdo da documentação, sempre que possível. Esta recomendação torna a documentação mais flexível e extensível para versões futuras.

### Uso de Day, AEM, CQ, CRX

Em um artigo, sempre faça referência ao produto pelo seu nome completo **Adobe Experience Manager** na primeira vez em que for usado. Consequentemente, ele pode ser referido como **AEM**.

Não se deve usar Day, Day Software, CQ e CRX, exceto quando for inevitável, como em nomes de classe ou em referência ao histórico do AEM.


