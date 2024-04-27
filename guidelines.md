---
source-git-commit: 2d90738d01fef6e37a2c25784ed4d1338c037c23
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 42%

---
# Diretrizes para colaboração na documentação do Adobe Experience Manager

## Filosofia da documentação

Os usuários do Adobe Experience Manager trabalham em ambientes extremamente competitivos, esforçando-se para criar experiências digitais que as destaquem de seus concorrentes. Portanto, quando a Adobe oferece novas ferramentas avançadas no AEM, essas ferramentas são complementadas com documentação precisa e transparente para permitir que os clientes usem imediatamente seu investimento no AEM e maximizem seu retorno sobre o investimento.

O objetivo é colocar a documentação do AEM nas mãos de seus usuários assim que possível. Portanto, o Adobe prioriza uma documentação precisa e utilizável, e se esforça para atualizá-la e aprimorá-la continuamente.

## Contribuições à documentação

Para melhorar continuamente a documentação do AEM, toda a comunidade de usuários do AEM é bem-vinda para contribuir em sua elaboração. Seja por meio de pull requests ou problemas, as melhorias na documentação podem ser correções, esclarecimentos, expansões e outros exemplos.

## Padrões de documentação

Embora o Adobe receba contribuições para sua documentação, qualquer contribuição para a documentação do AEM, na forma de um pull request ou um problema, deve estar em conformidade com os padrões de contribuição e documentação do Adobe.

As contribuições que não cumprirem esses padrões poderão ser rejeitadas.

### O Adobe documenta casos de uso padrão.

A documentação do AEM abrange casos de uso padrão. Casos de uso que não se enquadrarem no escopo de instalação e de uso padrão do produto não farão parte da documentação do AEM.

### O Adobe geralmente não documenta bugs e suas soluções.

A documentação do AEM abrange casos de uso padrão. Por essa razão, os bugs, seus efeitos e soluções alternativas não estão documentados.

As exceções a essa regra aplicam-se às notas de versão, nas quais problemas conhecidos podem ser listados com possíveis soluções que foram aprovadas pelo Gerenciamento de produtos do AEM.

### As contribuições à documentação não se destinam a responder perguntas técnicas.

Quaisquer ideias para melhorar a documentação do AEM são bem-vindas como contribuições. No entanto, comentários, problemas e pull requests destinam-se a *contribuições* somente. Eles não são para responder suas perguntas sobre como usar o AEM, implementar seu projeto AEM ou resolver problemas técnicos.

Você pode relatar qualquer dúvida sobre o uso do AEM ou erros técnicos usando o processo normal de suporte por meio da [Portal de suporte corporativo Experience Cloud](https://experienceleague.adobe.com/pt-br?support-solution=General#support) ou discutido na [comunidade do Experience Manager](https://experienceleaguecommunities.adobe.com/t5/adobe-experience-manager/ct-p/adobe-experience-manager-community?profile.language=pt).

***As contribuições à documentação do AEM não substituem o Atendimento ao cliente do Adobe*** e quaisquer contribuições que buscam respostas para perguntas relacionadas a suporte são rejeitadas.

### As contribuições devem mencionar claramente as páginas pertinentes à documentação.

Se você criar um problema para sugerir melhorias na documentação, deverá incluir links para as páginas afetadas. Se você criar um problema usando a variável **Editar esta página** em uma página de documentação, o problema é criado automaticamente com um link para a página.

Isso não se aplica a pull requests, uma vez que já fazem referência às páginas afetadas.

## Diretrizes de documentação

Adobe solicita que qualquer contribuição à sua documentação siga determinadas diretrizes de estilo.

Seguir essas diretrizes facilita a revisão de sua contribuição e, portanto, agiliza a integração à documentação do Adobe.

### Idioma e estilo

#### Idioma

* A documentação do AEM foi criada e mantida em inglês americano.
* Mantenha as sentenças o mais simples possível.
* Use linguagem clara e concisa.

Lembre-se de que os leitores da documentação do AEM estão espalhados ao redor do mundo, e não espera-se que sejam falantes nativos ou fluentes em inglês. Evite linguagem coloquial, mantendo-a a mais clara e simples possível.

#### Siga o Manual de estilo da Microsoft®

[Manual de estilo da Microsoft®](https://learn.microsoft.com/en-us/style-guide/welcome/) O é um guia de estilo disponível gratuitamente. Ele se concentra na documentação de softwares, e a documentação do AEM o segue sempre que possível.

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

Evite referências diretas a uma versão específica em todo o conteúdo da documentação, sempre que possível. Isso torna a documentação mais flexível e extensível para versões futuras.

### Uso do Day, AEM, CQ, CRX

Sempre use o nome completo do produto **Adobe Experience Manager** pela primeira vez num artigo e pode, em seguida, ser referido como **AEM**.

Não se deve usar Day, Day Software, CQ e CRX, exceto quando for inevitável, como em nomes de classe ou em referência ao histórico do AEM.