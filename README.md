## Cadastro: Importação automática de fornecedores via Excel

### 🛠 Problema real
O cadastro de fornecedores no módulo SA2 do Protheus é feito manualmente, campo por campo. Quando o volume de novos parceiros aumenta — como em eventos, campanhas de compras ou integração de filiais — o processo se torna **demorado, repetitivo e sujeito a erros**.

### 📉 Impacto
- Equipe de compras sobrecarregada
- Erros de digitação em dados fiscais, bancários ou contato
- Dificuldade em manter o sistema atualizado rapidamente
- Risco de atrasos no processo de compras e pagamentos

### 💡 Solução aplicada
Desenvolvimento de uma rotina que **importa dados direto de uma planilha Excel (.CSV)** e cadastra automaticamente os fornecedores na tabela SA2 (cadastro de fornecedores do Protheus).

A solução pode ser executada via rotina customizada (ADVPL) ou pelo `StartJob` com chamadas agendadas, dependendo do cenário da empresa.

### 🧾 Estrutura da planilha utilizada
| CNPJ         | Nome Fornecedor      | Endereço        | Cidade     | UF | CEP      | Telefone     | E-mail               |
|--------------|----------------------|------------------|-------------|----|-----------|---------------|------------------------|
| 00.000.000/0001-91 | Fornecedor Exemplo 1 | Rua A, 123       | São Paulo | SP | 01000-000 | (11) 90000-0001 | exemplo@forn.com.br |

> *Outros campos como IE, dados bancários e tipo de fornecedor podem ser adicionados.*

### 🧾 Trecho do código ADVPL (resumo)
```advpl
User Function ImporSA2()
    Local aDados := ReadCSV("C:\dados\fornecedores.csv")
    Local i := 0

    For i := 1 To Len(aDados)
        RecLock("SA2", .T.)
            SA2->A2_CGC     := aDados[i][1]
            SA2->A2_NOME    := aDados[i][2]
            SA2->A2_END     := aDados[i][3]
            SA2->A2_MUN     := aDados[i][4]
            SA2->A2_EST     := aDados[i][5]
            SA2->A2_CEP     := aDados[i][6]
            SA2->A2_TEL     := aDados[i][7]
            SA2->A2_EMAIL   := aDados[i][8]
        MsUnlock()
    Next

    MsgInfo("Importação concluída com sucesso.")
Return
