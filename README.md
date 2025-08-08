# LiteNet2 - Exemplo de Integração

Aplicação **C# ConsoleApp** de exemplo para integração com a placa **Toletus LiteNet2**.

---

## Recursos

- **Software Gerenciador Toletus LiteNet2:**  
  [📥 Download do Gerenciador LiteNet2](https://generic-spaces.actuar.cloud/suporte/Gerenciador%20Litenet%202.rar)

- **Manual de Integração da Placa Toletus LiteNet2:**  
  [📄 Manual de Integração no GitHub](https://github.com/Toletus/LiteNet2-ManuaisDeIntegracao)

---

# Mudança na resposta ao comando `prc`(procura)  – Versão V2.3.1 R0

A partir da versão **V2.3.1 R0**, o retorno à aplicação (`response` da catraca) ao comando `prc`(procura) `request` passou a incluir o **IP do computador conectado**.

## Formato Anterior
```text
TOLETUS LiteNet2@[ID]
```

## Novo Formato
```text
TOLETUS LiteNet2@[ID] c=[IP do computador conectado]
```

- Se o dispositivo estiver **conectado**, será exibido o respectivo **IP**.
- Caso contrário, será exibida a palavra: **none**
---

## Exemplo no Código

Na classe `LiteNetUtil`, método `OnUdpResponse`:

1. **Captura do ID**
    - Na primeira linha, é utilizada uma expressão regular (`regex`) que busca na variável `device` um padrão contendo `@` seguido de dígitos (`\d+`).
    - Esses dígitos são capturados para posterior processamento.

2. **Conversão para UInt16**
    - Na segunda linha, os dígitos capturados são extraídos e convertidos para um número inteiro de 16 bits (`Int16`), e em seguida para `UInt16`.
    - Isso permite identificar corretamente o **ID do dispositivo**.

```csharp
// Captura os dígitos após o '@'
var match = Regex.Match(device, @"@(\d+)");
var id = (UInt16)Convert.ToInt16(m.Groups[1].Value);

```

---

## Exemplos de Retorno

| Situação                  | Retorno                                          | Versão       | 
|---------------------------|--------------------------------------------------|--------------|
| Dispositivo conectado     | `TOLETUS LiteNet2@12 c=192.168.0.10`              | V2.3.1 R0    |
| Dispositivo desconectado  | `TOLETUS LiteNet2@12 c=none`                      | V2.3.1 R0    |
| Dispositivo conectado ou  desconectado| `TOLETUS LiteNet2@12`                      | <= V2.2.2 R0 |
