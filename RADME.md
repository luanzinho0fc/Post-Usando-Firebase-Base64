# Upload de Imagens com Firebase Firestore e Base64

Este projeto permite o envio e armazenamento de imagens em uma aplicação web, com a capacidade de exibir as imagens e gerar links permanentes para elas, utilizando o Firebase Firestore e a conversão de imagens para o formato Base64. A solução foi desenvolvida de forma eficiente e econômica, evitando custos associados ao Firebase Storage.

## Índice

- [Visão Geral](#visão-geral)
- [Como Funciona](#como-funciona)
  - [Fluxo de Funcionamento](#fluxo-de-funcionamento)
  - [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Vantagens](#vantagens)
- [Implementação](#implementação)
  - [Estrutura do Código](#estrutura-do-código)
  - [Funcionamento Detalhado](#funcionamento-detalhado)
- [Considerações Finais](#considerações-finais)
- [Limitações](#limitações)
- [Licença](#licença)

## Visão Geral

Este projeto permite a conversão de imagens para o formato Base64 e o armazenamento dessas imagens no Firebase Firestore, um banco de dados NoSQL da Google. Ao invés de utilizar o Firebase Storage, que pode gerar custos, a solução usa o Firestore para armazenar a imagem como uma string Base64. Essa abordagem economiza recursos enquanto permite o armazenamento e a recuperação das imagens de forma eficiente.

## Como Funciona

### Fluxo de Funcionamento

1. **Seleção da Imagem**: O usuário escolhe uma imagem utilizando um campo de input de arquivo.
2. **Conversão para Base64**: A imagem selecionada é convertida em uma string Base64 utilizando a API `FileReader`.
3. **Salvamento no Firestore**: A string Base64 e um identificador único gerado aleatoriamente são salvos no Firestore.
4. **Exibição da Imagem**: Após o upload, a imagem pode ser exibida na página utilizando a string Base64. O link gerado também é mostrado para o usuário.
5. **Recuperação das Imagens**: Ao carregar a página, o sistema recupera as imagens do Firestore e exibe-as na interface.

### Tecnologias Utilizadas

- **Firebase Firestore**: Banco de dados NoSQL utilizado para armazenar as imagens codificadas em Base64.
- **Base64**: Método utilizado para converter imagens em strings, permitindo o armazenamento eficiente no Firestore.
- **JavaScript**: Linguagem de programação utilizada para a lógica de manipulação das imagens e integração com o Firestore.
- **HTML & CSS**: Utilizados para estruturar e estilizar a interface de usuário.

## Vantagens

- **Baixo Custo**: Armazenar imagens como strings Base64 no Firestore evita custos com o Firebase Storage.
- **Simplicidade**: A solução é fácil de implementar e não requer configuração complexa.
- **Links Permanentes**: Cada imagem salva no Firestore tem um identificador único, garantindo links permanentes e reutilizáveis.
- **Escalabilidade Limitada**: Ideal para projetos pequenos e médios com um número reduzido de imagens.

## Implementação

### Estrutura do Código

O código é composto por três componentes principais:

1. **HTML**: Estrutura da página que inclui o campo de upload de arquivos, botão de envio e área para exibir as imagens.
2. **CSS**: Estilização simples da página para exibir as imagens de forma organizada.
3. **JavaScript**: Lógica responsável pela conversão da imagem para Base64, upload para o Firestore e exibição das imagens.

#### Estrutura do Firestore

- **Coleção**: `images`
- **Campos**:
  - `imageBase64`: A string Base64 da imagem.
  - `imageLink`: Link único gerado para identificar a imagem.

### Funcionamento Detalhado

1. **Seleção de Imagem**: O usuário clica no botão "Selecionar Imagem" e escolhe um arquivo.
2. **Conversão para Base64**: A imagem é lida como um arquivo binário e convertida para uma string Base64 utilizando o `FileReader`.
3. **Geração do Link Aleatório**: Um identificador aleatório é gerado para associar à imagem, criando um link único.
4. **Upload no Firestore**: A imagem codificada em Base64 e o link gerado são armazenados no Firestore.
5. **Exibição na Página**: Após o upload, a imagem é exibida na página utilizando a string Base64. O link gerado também é mostrado, permitindo o compartilhamento ou acesso à imagem posteriormente.

```javascript
const uploadImage = () => {
  const file = document.getElementById("image-input").files[0];
  const reader = new FileReader();

  reader.onloadend = () => {
    const base64String = reader.result.split(",")[1];
    const imageLink = generateLink();
    saveToFirestore(base64String, imageLink);
  };

  reader.readAsDataURL(file);
};
