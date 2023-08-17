# Distribuição LeadSource

**Salesforce Lead Distribution ReadMe**

Este ReadMe explica um exemplo de código que demonstra como criar um acionador (trigger) no Salesforce para distribuir automaticamente os leads com a fonte "formulario" para os usuários de atendimento, com base no campo personalizado "CANAL__c" dos usuários.

**Objetivo:**
Distribuir automaticamente leads com fonte "formulario" para usuários de atendimento, utilizando um acionador e uma classe assíncrona no Salesforce.

**Passos:**

1. **Classe `Distribuir`:**

    - `@future`: Este atributo indica que o método `distribuirUsuariosAsync` será executado de forma assíncrona em uma chamada futura.
    - `public static void distribuirUsuariosAsync(List<Id> leadIds)`: Este método assíncrono recebe uma lista de IDs de leads como parâmetro.

2. **Acionador `LeadTrigger`**:

    - `trigger LeadTrigger on Lead (after insert) { ... }`: Este acionador é acionado após a inserção de registros do objeto `Lead`.
    - Ele filtra os leads com a fonte "formulario" e, se existirem, cria uma chamada futura para a classe `Distribuir` para distribuir os usuários de atendimento.

**Explicação Detalhada:**

- O acionador `LeadTrigger` é acionado após a inserção de novos registros no objeto `Lead`.
- O acionador verifica cada novo lead para verificar se a fonte é "formulario".
- Se o lead tiver a fonte "formulario", ele é adicionado a uma lista `formularioLeads`.
- Se a lista `formularioLeads` não estiver vazia, o acionador cria uma lista `leadIds` para armazenar os IDs dos leads.
- O acionador chama o método assíncrono `distribuirUsuariosAsync` da classe `Distribuir`, passando a lista `leadIds` como parâmetro.
- Na classe `Distribuir`, o método `distribuirUsuariosAsync` recebe a lista `leadIds`.
- Ele consulta os usuários de atendimento com base no campo personalizado "CANAL__c".
- Para cada lead, ele calcula um índice com base no ID do lead e escolhe um usuário de atendimento.
- Ele cria um novo objeto `Lead` com o ID do lead e atualiza o proprietário para o usuário de atendimento escolhido.
