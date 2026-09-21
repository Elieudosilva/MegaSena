# 🎰 Mega-Sena Compose

Aplicativo Android desenvolvido com **Kotlin** e **Jetpack Compose** para geração de apostas da Mega-Sena.

O usuário informa a quantidade de números que deseja gerar, entre 6 e 15 dezenas, e o aplicativo cria uma combinação aleatória utilizando números de 1 a 60.

A aplicação também utiliza `SharedPreferences` para armazenar a última combinação gerada, permitindo que o resultado permaneça disponível mesmo após o aplicativo ser fechado e aberto novamente.

---

## 📋 Sobre o projeto

O **Mega-Sena Compose** foi desenvolvido como um projeto de estudo e prática de desenvolvimento Android moderno utilizando Jetpack Compose.

A proposta é criar uma aplicação simples, mas que permita trabalhar conceitos importantes do desenvolvimento Android, como:

- construção de interfaces declarativas com Jetpack Compose;
- gerenciamento de estado da interface;
- validação de dados de entrada;
- geração de valores aleatórios;
- persistência local de dados;
- utilização de componentes do Material 3;
- criação de previews para a interface.

A aplicação possui uma tela principal onde o usuário informa a quantidade de números desejada e, após validar a entrada, pode gerar uma nova combinação.

---

## 📸 Screenshots

<!-- Substitua os caminhos das imagens abaixo pelos screenshots reais do projeto -->

| Tela principal | Resultado gerado |
|:---:|:---:|
<p align="center">
   <img src="https://github.com/user-attachments/assets/0649296d-4132-4bb0-8e14-e6f7ebe1072f" alt="Screen_one" width="200"/>
   <img src="https://github.com/user-attachments/assets/3eb9a82e-6c36-4410-8593-bae954d4570d" alt="Screen_two" width="200"/>

   </p>

---

## 🎥 Demonstração

Confira o aplicativo em funcionamento demonstrando a entrada da quantidade de números, a validação dos dados, a geração da aposta e a persistência do último resultado.

<!-- Substitua SEU_LINK_DO_VIDEO pelo link do vídeo -->

<video src="SEU_LINK_DO_VIDEO" controls="controls" style="max-width: 100%; height: auto;">
  Seu navegador não suporta a reprodução do vídeo.
</video>

---

## ✨ Funcionalidades

- **Geração de apostas:** cria combinações aleatórias utilizando números de 1 a 60.
- **Quantidade personalizada:** permite gerar apostas com 6 a 15 números.
- **Validação de entrada:** impede valores vazios ou fora do intervalo permitido.
- **Filtro de caracteres:** o campo aceita somente caracteres numéricos.
- **Exibição do resultado:** apresenta a combinação gerada diretamente na tela.
- **Persistência local:** salva a última combinação utilizando `SharedPreferences`.
- **Recuperação do último resultado:** ao iniciar a tela, o último resultado armazenado é carregado.
- **Feedback de erro:** apresenta uma mensagem ao usuário quando a quantidade informada é inválida.
- **Interface declarativa:** toda a interface principal foi construída utilizando Jetpack Compose.
- **Preview:** utilização do `@Preview` para visualizar a interface durante o desenvolvimento.

---

## 🛠️ Tecnologias utilizadas

| Tecnologia | Aplicação no projeto |
|---|---|
| **Kotlin** | Linguagem principal do aplicativo. |
| **Jetpack Compose** | Construção da interface de usuário. |
| **Material 3** | Componentes visuais e tema da aplicação. |
| **Compose State** | Gerenciamento do estado dos campos e do resultado. |
| **SharedPreferences** | Persistência local da última aposta gerada. |
| **Kotlin Random** | Geração aleatória das dezenas. |
| **Android Toast** | Exibição de mensagens de validação para o usuário. |

---

## 🏗️ Estrutura da aplicação

A implementação possui uma estrutura enxuta, concentrando a tela principal e a lógica de geração/validação em funções Kotlin e separando a persistência local em uma classe própria.

```
app/src/main/java/co/tiagoaguiar/megasenacomposedev/
├── MainActivity.kt
└── PreferencesManager.kt
```

### MainActivity

É responsável por iniciar a aplicação e disponibilizar a interface construída com Jetpack Compose.

O `MainApp()` concentra a tela principal e mantém os estados necessários para:

- valor digitado pelo usuário;
- resultado da última aposta;
- atualização da interface após uma nova geração.

### PreferencesManager

Responsável pela persistência da última aposta utilizando `SharedPreferences`.

A classe disponibiliza duas operações principais:

```kotlin
saveData(key: String, value: String)
```

Utilizada para armazenar o resultado gerado.

```kotlin
getData(key: String): String
```

Utilizada para recuperar o resultado salvo anteriormente.

Essa separação evita que a lógica de persistência fique diretamente misturada com a implementação da interface.

---

## 🔢 Geração dos números

A geração das dezenas é realizada pela função:

```kotlin
fun numberGenerator(qtd: Int): String
```

O método utiliza `Random.nextInt(60)` para produzir valores entre 0 e 59. Em seguida, adiciona 1 ao valor gerado para obter números entre 1 e 60.

```kotlin
val n = Random.nextInt(60)
numbers.add(n + 1)
```

A quantidade de números gerados é determinada pelo valor informado pelo usuário.

O resultado final é convertido para uma `String` utilizando:

```kotlin
numbers.joinToString(" - ")
```

Por exemplo:

```
04 - 17 - 23 - 31 - 42 - 58
```

### ⚠️ Observação sobre a geração

A implementação atual utiliza uma `MutableList` e não verifica se um número já foi sorteado anteriormente.

Consequentemente, podem ocorrer números repetidos dentro de uma mesma aposta.

Para representar uma aposta tradicional da Mega-Sena, uma evolução possível seria garantir que todas as dezenas sejam únicas antes de apresentar o resultado.

---

## ✅ Validação da entrada

A entrada do usuário passa por duas etapas.

### 1. Filtragem dos caracteres

A função `validateInput()` remove caracteres que não sejam números:

```kotlin
fun validateInput(input: String): String {
    val filteredCharsinput = input.filter {
        it in "0123456789"
    }

    return filteredCharsinput
}
```

Dessa forma, o campo mantém somente caracteres numéricos.

### 2. Validação da quantidade

Depois, `validateTextField()` verifica se o valor informado está dentro do intervalo permitido:

```kotlin
fun validateTextField(text: String): Boolean
```

A aplicação considera válido somente um valor entre 6 e 15.

Caso o valor seja inválido, o usuário recebe um `Toast` informando:

> Digite número entre 6 e 15!

---

## 💾 Persistência do resultado

A última combinação gerada é armazenada localmente utilizando `SharedPreferences`.

O fluxo funciona da seguinte maneira:

```
Usuário informa a quantidade
          ↓
Validação da entrada
          ↓
Geração dos números
          ↓
Atualização do estado da interface
          ↓
Persistência com SharedPreferences
```

Ao criar a tela novamente, o aplicativo recupera o valor anteriormente salvo:

```kotlin
val result = remember {
    mutableStateOf(
        prefs.getData(PREFS_KEY)
    )
}
```

Isso permite que o último resultado continue disponível após o encerramento e posterior abertura do aplicativo.

---

## 🧠 Gerenciamento de estado

A interface utiliza o mecanismo de estado do Jetpack Compose:

```kotlin
remember {
    mutableStateOf(...)
}
```

O estado é utilizado principalmente para controlar:

- o conteúdo digitado no campo de entrada;
- o resultado apresentado na tela.

Quando o valor de `result` é alterado após uma nova geração:

```kotlin
result.value = numberGenerator(bet.value.toInt())
```

o Compose detecta a alteração e recompõe a interface que depende desse estado.

Esse comportamento permite construir uma interface reativa sem a necessidade de atualizar manualmente os componentes visuais.

---

## 🎨 Interface

A interface foi construída utilizando componentes do Jetpack Compose e Material 3.

Entre os componentes utilizados estão:

- `Surface`
- `Column`
- `Text`
- `TextField`
- `Button`

O layout utiliza `Arrangement` e `Alignment` para organizar os elementos verticalmente e centralizá-los na tela.

O título **"Boa Sorte!"** utiliza uma cor verde e uma tipografia destacada para reforçar a identidade visual da aplicação.

---

## 🚧 Desafios técnicos e aprendizados

### 1. Trabalhando com estado no Compose

**Desafio:** atualizar o resultado exibido na tela sempre que uma nova aposta fosse gerada.

**Solução:** utilização de `mutableStateOf` dentro de `remember`.

**Aprendizado:** no Jetpack Compose, a interface pode ser construída diretamente a partir do estado da aplicação, permitindo que alterações nos dados provoquem automaticamente a recomposição dos componentes envolvidos.

### 2. Validação de entrada

**Desafio:** evitar que o usuário informasse valores inválidos ou caracteres que não representassem uma quantidade numérica.

**Solução:** combinação de `validateInput()` para filtrar os caracteres e `validateTextField()` para validar o intervalo de 6 a 15.

**Aprendizado:** validar a entrada próxima ao ponto de interação melhora a previsibilidade do comportamento da aplicação e evita que valores inadequados cheguem à lógica de geração.

### 3. Persistência local

**Desafio:** manter a última aposta disponível mesmo depois que a aplicação fosse encerrada.

**Solução:** criação da classe `PreferencesManager`, responsável por encapsular o acesso ao `SharedPreferences`.

**Aprendizado:** isolar a persistência em uma classe própria mantém a interface focada apenas na exibição e facilita futuras trocas da estratégia de armazenamento.
