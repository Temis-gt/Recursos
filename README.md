# ⚒️🔨 Assets / Recursos 🏹🪓
Recursos del cliente y servidor de Argentum Online

# Important Notice:
Before utilizing these assets, it is crucial that you thoroughly review the license terms. For detailed information on usage rights and restrictions, please consult the [license.txt](license.txt) file. Your understanding and adherence to these terms ensure the respectful and lawful use of these resources.

# What are we not sharing?
Although configuration files (such as DATs, INIs, graphics, etc.) can be freely adjusted by anyone, the resource repository is fully functional to ensure all game features work smoothly. However, configuration values, like object positions on maps, weapon statistics, and spell balance, can be customized. This allows for flexibility while maintaining the core structure of the game intact.

## Por favor considera apoyarnos en https://www.patreon.com/nolandstudios

# Visual Basic 6 Code Standards (Argentum Online)

## ✅ How to Use This with ChatGPT (or any coding AI)

### ✳️ What is this?
This is a document with **context and coding rules** to help the AI understand how we code in **Argentum Online using Visual Basic 6**.  
AI doesn’t guess well — if you don’t give it proper instructions, it will return incorrect, modern, or inconsistent code.

---
### ✅ Step-by-step (to make the AI behave):
- First, copy and paste the Context into the chat. This tells the AI exactly what kind of project it’s working on and how it should think.
- Once the AI responds or acknowledges, paste the VB6 coding rules so it knows how to format and structure the code correctly.
- Now you're ready to ask for code improvements, refactors, or new implementations — and the AI will (usually) follow the correct style.
---

#### 1. **Copy and paste this context** at the start of your chat (just once per session):

```
You are an experienced Visual Basic 6 developer working on the legacy MMORPG Argentum Online.
The project uses a client-server architecture, where both the server and the client are written in VB6.
Communication between them occurs via a custom packet protocol.
All development must follow legacy-compatible practices and the official coding standards.
Prioritize clean, readable code with minimal risk of regression.
Add comments to the code to make it understandable.
Avoid modern syntax not supported by VB6. Follow the standards in this document.
```

#### 2. **Then paste the coding rules** from this file, they are below in this document

#### 3. **Only then**, ask your question:

> 🛠️ Example of a good prompt:
>
> ```
> Context: [paste the text from step 1]
>
> These are our coding rules: [paste rules from this document]
>
> Now, can you rewrite this function to follow those standards?
> [paste your VB6 function]
> ```

---

### 💬 Why do it this way?

Because if you don’t:
- The AI might write VB.NET or modern syntax that VB6 doesn’t support
- It may ignore the style rules used in Argentum Online
- You’ll waste time correcting formatting or compatibility issues

--- 

## Coding Rules

### Repositories

- **Server repository**: [ao-org/argentum-online-server](https://github.com/ao-org/argentum-online-server)
- **Client repository**: [ao-org/argentum-online-client](https://github.com/ao-org/argentum-online-client)

# Coding Standards

## 1. Mandatory use of `Call` and parentheses

* Always use `Call` when invoking any `Sub`.
* Always include parentheses, even if there are no arguments.

```vb
Call GuardarDatos()
Call EnviarMensaje("Hola", 2)
```

## 2. Functions always with parentheses

* Even when ignoring the return value, functions must be called with parentheses.

```vb
Call ObtenerTiempoActual()
Dim puntos As Long
puntos = CalcularPuntos(usuarioId)
```

## 3. Naming conventions

| Element        | Convention              | Example                   |
| -------------- | ----------------------- | ------------------------- |
| Modules        | `mod` prefix            | `modNetwork`, `modLogin`  |
| Forms          | `frm` prefix            | `frmMain`, `frmLogin`     |
| Controls       | Hungarian notation      | `txtNombre`, `lblError`   |
| Variables      | `camelCase`             | `userIndex`, `goldAmount` |
| Constants      | `UPPER`                 | `GOLD_PRICE`              |
| Functions/Subs | `PascalCase`            | `Call ValidarUsuario()`   |
| Enums          | `e_` prefix             | `e_TipoPago`              |

## 4. Spacing and formatting

* Use 4-space indentation.
* One blank line between procedures.
* Align related variable declarations:

```vb
Dim userGold    As Long
Dim userSilver  As Long
Dim userName    As String
```

## 5. Avoid magic numbers

* Declare all fixed values as constants in shared modules.

```vb
Public Const GOLD_PRICE As Long = 50000

If .Stats.GLD < GOLD_PRICE Then
    Call EscribirError("No tenés suficiente oro.")
End If
```

## 6. Standardized error handling

* Always use `On Error GoTo Name_Err` with a label at the end of the `Sub`.

```vb
Private Sub ValidarSesion()
    On Error GoTo ValidarSesion_Err

    Call HacerAlgo()

    Exit Sub

ValidarSesion_Err:
    Call TraceError(Err.Number, Err.Description, "modLogin.ValidarSesion", Erl)
End Sub
```

## 7. Explicit identifiers

* Use clear, specific names.
* Avoid generic names like `dato`, `res`, `temp`.

```vb
Dim creditAmount As Long
Dim connectionId As Integer
```

## 8. SQL queries

* Always use parameterized queries (`?`).
* Always close all `Recordset` objects.

```vb
Dim RS As ADODB.Recordset
Set RS = Query("SELECT nivel FROM user WHERE id = ?;", userId)

If Not RS.EOF Then
    nivel = RS!nivel
End If

Call RS.Close
```

## 9. Localized messages

### Server-Side Localised Messaging

On the server, we use **message IDs** to send localised messages to clients. This approach allows for easy translation and dynamic content insertion.

#### How It Works

1. The server sends a message by referencing a **message ID**.
2. The client resolves that ID using a message index file provided in the [`argentum-online-creador-indices`](https://github.com/ao-org/argentum-online-creador-indices) repository.
3. These message definitions are part of the assets system.

#### 💬 Example

```vb
Call WriteLocaleMsg(UserIndex, "1291", FONTTYPE_INFOBOLD, GOLD_PRICE)
```

- `"1291"` refers to a message like:  
  `"You need at least ¬1 gold to sell your character"`
- `GOLD_PRICE` dynamically replaces `¬1` in the final message shown to the user.

#### Message Files

- [Spanish messages – `SP_LocalMsg.dat`](https://github.com/ao-org/Recursos/blob/master/init/SP_LocalMsg.dat)
- [English messages – `EN_LocalMsg.dat`](https://github.com/ao-org/Recursos/blob/master/init/EN_LocalMsg.dat)

### Client-side (`JsonLanguage`)

For messages handled purely on the **client side**, we use **JSON files for translation**, located in the [`Languages`](https://github.com/ao-org/argentum-online-client/tree/master/Languages) folder of the client repository:

* [Languages/1.json (Spanish)](https://github.com/ao-org/argentum-online-client/blob/master/Languages/1.json)
* [Languages/2.json (English)](https://github.com/ao-org/argentum-online-client/blob/master/Languages/2.json)

Example:

```vb
Call MsgBox(JsonLanguage.Item("MENSAJE_ERROR_CARGAR_OPCIONES"), vbCritical, JsonLanguage.Item("TITULO_ERROR_CARGAR"))
```

This allows full client-side UI translation and supports switching languages at runtime by loading the appropriate JSON file.

## 10. Clear control flow

* Use `Exit Sub` for early exits.
* Avoid unnecessary nested structures.

```vb
If Not EstaAutenticado(UserIndex) Then
    Call Desconectar(UserIndex)
    Exit Sub
End If
```

## 11. Pure functions and no side effects

* Prefer `Function` for validations or transformations.
* Avoid modifying global variables unnecessarily.
