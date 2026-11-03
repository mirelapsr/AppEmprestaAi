# Empresta Aí
Especificação v1  
Mirela | Data: 03/11/2026

---

## 1. Visão
Pessoas que costumam emprestar objetos pessoais (como livros, ferramentas e jogos) para amigos frequentemente perdem o controle do paradeiro desses pertences ou do prazo de devolução. O **Empresta Aí** é um aplicativo mobile focado no Dono do objeto que visa organizar e registrar empréstimos, controlar prazos e notificar sobre pendências. Com o app, o usuário obtém histórico transparente de empréstimos, minimiza perdas permanentes e mantém uma relação saudável e organizada com seus contatos.

---

## 2. Escopo

### 2.1 Dentro do escopo
1. Cadastrar objetos com nome, categoria, foto opcional e estado de conservação inicial.
2. Cadastrar amigos/contatos com nome, telefone e e-mail (opcional).
3. Registrar empréstimo associando um objeto disponível a um amigo cadastrado, definindo data de saída e prazo de devolução.
4. Renovar empréstimo ativo, alterando a data limite de devolução.
5. Registrar devolução de objeto, especificando a data real de retorno e o estado de conservação final do item.
6. Registrar perda ou não devolução de objeto.
7. Listar e filtrar empréstimos por status: Ativos, Atrasados e Encerrados.
8. Enviar notificações para o Dono sobre prazos prestes a vencer e atrasos consolidados.
9. Consultar histórico completo de movimentações por Objeto e por Amigo.

### 2.2 Fora do escopo (Limitações explícitas)
1. **Cobrança ou pagamento real de multas, aluguéis ou indenizações:** O app não realiza transações financeiras.
2. **Marketplace público de objetos:** Não há compartilhamento ou busca de objetos entre desconhecidos ou rede pública.
3. **Chat/mensageria entre usuários:** O app não possui sistema interno de troca de mensagens.
4. **Rastreamento por geolocalização:** Não há uso de GPS nem rastreamento físico de objetos ou pessoas.

---

## 3. Glossário

* **Dono:** Único usuário autenticado do aplicativo, proprietário dos objetos e gerenciador do acervo.
* **Tomador:** Pessoa (amigo/contato) cadastrada pelo Dono que recebe um objeto emprestado. Não possui conta nem acesso ao app.
* **Objeto:** Item individual ou conjunto indivisível cadastrado pelo Dono.
* **Empréstimo Ativo:** Registro em que o objeto foi entregue ao Tomador, o prazo limite de devolução é igual ou posterior à data atual (considerando o fuso horário America/Sao_Paulo) e a devolução ainda não foi confirmada.
* **Atrasado:** Registro em que a devolução não ocorreu e a data atual é posterior ao prazo limite de devolução (a partir das 00:00:00 do dia seguinte ao vencimento).
* **Encerrado:** Estado final do empréstimo após o registro de devolução concluída ou confirmação de perda.
* **Renovação:** Ato de estender a data de devolução limite de um empréstimo ativo ou atrasado.

---

## 4. Atores e permissões

| Ator | Pode fazer | Não pode fazer |
| :--- | :--- | :--- |
| **Dono** | • Cadastrar, editar e remover objetos e amigos.<br>• Criar, renovar, encerrar e marcar perdas em empréstimos.<br>• Visualizar relatórios, notificações e históricos. | • Alterar registros de empréstimos já encerrados.<br>• Excluir contatos/objetos vinculados a empréstimos ativos/atrasados. |
| **Tomador** | • Receber comunicações externas do Dono (fora do app). | • Acessar o sistema, visualizar telas ou interagir diretamente com a interface do aplicativo. |

---

## 5. Regras de negócio

### Gestão de Objetos e Amigos
* **RN01:** Um objeto pode ter no máximo **1 empréstimo ativo ou atrasado** por vez.
* **RN02:** Um conjunto de itens agrupados (ex: "Coleção Harry Potter 7 Livros") deve ser cadastrado como um **único objeto** no sistema.
* **RN03:** Não é permitida a exclusão de um objeto ou amigo que possua histórico de empréstimo (ativo, atrasado ou encerrado). Nestes casos, o item ou amigo só pode ser arquivado/desativado.
* **RN04:** A remoção completa de dados de um amigo (conforme LGPD) apaga seus dados pessoais (nome, telefone, email), mantendo o histórico de empréstimo com o tomador rotulado anonimamente como `"Contato Removido"`.

### Prazos e Vigência
* **RN05:** A data de devolução padrão é definida automaticamente por categoria, podendo ser sobrescrita manualmente no momento da criação:
  * Ferramentas: **7 dias**
  * Eletrônicos: **14 dias**
  * Livros/Jogos: **30 dias**
  * Outros: **15 dias**
* **RN06:** O prazo limite de devolução vence rigorosamente às **23:59:59 (horário de Brasília - UTC-3)** do dia estipulado.
* **RN07:** O prazo mínimo de um empréstimo é de 1 dia e o prazo máximo é de 365 dias. Empréstimos sem prazo definido não são permitidos.

### Atrasos e Notificações
* **RN08:** Um empréstimo é considerado **Atrasado** a partir da 00:00:00 do dia subsequente ao prazo de devolução. Não há período de tolerância.
* **RN09:** O app emitirá alertas locais ao Dono com a seguinte frequência:
  * **Aviso de Prevenção:** 1 dia antes do vencimento, às 09:00 AM.
  * **Aviso de Vencimento:** No dia do vencimento, às 09:00 AM.
  * **Aviso de Atraso:** Diariamente a partir do 1º dia de atraso até o 7º dia de atraso, às 10:00 AM. A partir do 8º dia, os avisos cessam.

### Renovações
* **RN10:** Apenas o Dono pode executar a renovação dentro do aplicativo.
* **RN11:** Cada empréstimo pode ser renovado no máximo **3 vezes**.
* **RN12:** A renovação pode ser feita em empréstimos com status `Ativo` ou `Atrasado`. A nova data de devolução é calculada somando os novos dias acordados a partir da data em que a renovação é realizada.

### Devoluções, Danos e Perdas
* **RN13:** Ao registrar a devolução, o Dono deve selecionar obrigatoriamente o estado de devolução entre as opções: `Sem Alteração`, `Dano Leve`, `Dano Grave`, `Inutilizado`.
* **RN14:** Caso o estado seja `Dano Grave` ou `Inutilizado`, o cadastro do objeto atualiza automaticamente seu estado de conservação atual para `Danificado` ou `Inutilizado`.
* **RN15:** Devolução parcial não encerra o empréstimo. O registro permanece `Ativo` ou `Atrasado` até que a totalidade do item seja devolvida ou o saldo pendente seja declarado como `Perdido`.
* **RN16:** Um empréstimo em atraso há mais de **30 dias** exibe um atalho para ser declarado como `Perdido` pelo Dono.
* **RN17:** Caso um objeto marcado como `Perdido` seja recuperado, o Dono pode registrar uma nova entrada de ajuste no histórico para reativar o objeto como `Disponível`.

---

## 6. Histórias de usuário

### HU01 - Cadastrar Objeto
**Como** Dono  
**Quero** cadastrar um objeto com suas características iniciais  
**Para** mantê-lo disponível em meu inventário de empréstimos.  
**Regras Relacionadas:** RN02, RN05, RN13

* **Cenário 01: Cadastro com sucesso**
  * **Dado** que estou na tela "Novo Objeto",
  * **Quando** preencho o Nome com "Furadeira de Impacto 500W", Categoria com "Ferramentas" e Estado de Conservação com "Bom",
  * **E** clico em "Salvar",
  * **Então** o objeto é salvo no banco de dados e exibido na lista de objetos com status "Disponível".

* **Cenário 02: Validação de campo obrigatório**
  * **Dado** que estou na tela "Novo Objeto",
  * **Quando** deixo o campo Nome em branco e clico em "Salvar",
  * **Então** o sistema exibe a mensagem de erro "O nome do objeto é obrigatório." e o registro não é finalizado.

---

### HU02 - Registrar Empréstimo
**Como** Dono  
**Quero** associar um objeto disponível a um amigo especificando a data limite  
**Para** ter o controle de para quem o item foi entregue e quando deve voltar.  
**Regras Relacionadas:** RN01, RN05, RN06, RN07

* **Cenário 01: Registrar empréstimo com prazo padrão**
  * **Dado** que tenho o objeto "Jogo Catan" (Categoria: Jogos) com status "Disponível" e o amigo "Carlos Silva" cadastrado,
  * **Quando** crio um empréstimo para "Carlos Silva" no dia 03/11/2026 sem alterar o prazo sugerido,
  * **Então** o sistema define a data de devolução para 03/12/2026 (30 dias), salva o empréstimo como "Ativo" e altera o status do objeto para "Emprestado".

* **Cenário 02: Tentativa de emprestar objeto já emprestado**
  * **Dado** que o objeto "Livro Clean Code" possui um empréstimo com status "Ativo",
  * **Quando** tento selecionar "Livro Clean Code" para criar um novo empréstimo,
  * **Então** o sistema impede a seleção e exibe a mensagem "Este objeto já está em um empréstimo ativo."

---

### HU03 - Renovar Empréstimo
**Como** Dono  
**Quero** estender a data limite de devolução de um empréstimo  
**Para** conceder mais tempo ao amigo sem marcar o item como atrasado.  
**Regras Relacionadas:** RN10, RN11, RN12

* **Cenário 01: Renovação válida dentro do limite**
  * **Dado** que um empréstimo possui 0 renovações registradas e prazo limite para 10/11/2026,
  * **Quando** clico em "Renovar", informando mais 7 dias no dia 08/11/2026,
  * **Então** a nova data limite é atualizada para 15/11/2026, o contador de renovações sobe para 1 e um registro é adicionado na tabela de Renovações.

* **Cenário 02: Limite de renovações atingido**
  * **Dado** que um empréstimo já possui 3 renovações cadastradas,
  * **Quando** visualizo os detalhes do empréstimo,
  * **Então** o botão "Renovar" é exibido desabilitado e a mensagem "Limite máximo de 3 renovações atingido" é apresentada.

---

### HU04 - Registrar Devolução
**Como** Dono  
**Quero** registrar a devolução de um objeto informando seu estado final  
**Para** encerrar o empréstimo e atualizar o status do meu objeto.  
**Regras Relacionadas:** RN13, RN14

* **Cenário 01: Devolução em perfeito estado**
  * **Dado** que o empréstimo do objeto "Martelo" está com status "Ativo",
  * **Quando** registro a devolução informando a data atual e seleciono o estado "Sem Alteração",
  * **Então** o status do empréstimo muda para "Encerrado", a data_devolucao é preenchida e o status do objeto "Martelo" volta para "Disponível".

---

## 7. Estados do empréstimo

| Estado Atual | Evento | Próximo Estado | Regra / Condição |
| :--- | :--- | :--- | :--- |
| *Nenhum* | Criar Empréstimo | **Ativo** | Objeto deve estar com status "Disponível". |
| **Ativo** | Data atual ultrapassa 23:59:59 do prazo | **Atrasado** | Processado automaticamente na virada do dia (RN08). |
| **Ativo** | Registrar Devolução | **Encerrado** | Preenchimento de `data_devolucao` e `estado_devolucao`. |
| **Ativo** | Solicitar Renovação | **Ativo** | Limite de renovações < 3 (RN11). Data limite é estendida. |
| **Atrasado** | Registrar Devolução | **Encerrado** | Preenchimento de `data_devolucao` e `estado_devolucao`. |
| **Atrasado** | Solicitar Renovação | **Ativo** | Limite de renovações < 3 (RN12). Status retorna para Ativo. |
| **Atrasado** | Marcar como Perdido | **Perdido** | Ação manual do Dono após avaliação (RN16). |

---

## 8. Modelo de dados

### 8.1 Tabela: `amigos`
Guarda as informações das pessoas para quem o Dono empresta pertences.

| Atributo | Tipo | Obrigatório? | Descrição / Chave |
| :--- | :--- | :--- | :--- |
| `id` | Número (INTEGER) | Sim | Chave Primária (Auto-incremental). |
| `nome` | Texto (VARCHAR 100) | Sim | Nome completo do amigo. |
| `telefone` | Texto (VARCHAR 20) | Sim | Telefone de contato. |
| `email` | Texto (VARCHAR 100) | Não | Endereço de e-mail opcional. |

### 8.2 Tabela: `objetos`
Guarda os itens cadastrados no inventário pessoal.

| Atributo | Tipo | Obrigatório? | Descrição / Chave |
| :--- | :--- | :--- | :--- |
| `id` | Número (INTEGER) | Sim | Chave Primária (Auto-incremental). |
| `nome` | Texto (VARCHAR 100) | Sim | Nome descritivo do item. |
| `categoria` | Texto (VARCHAR 50) | Sim | Categoria (Ferramentas, Livros, Eletrônicos, Jogos, Outros). |
| `foto_url` | Texto (VARCHAR 255) | Não | Caminho ou URL da foto do objeto. |
| `estado_conservacao` | Texto (VARCHAR 50) | Sim | Estado atual (Novo, Bom, Desgastado, Danificado, Inutilizado). |

### 8.3 Tabela: `emprestimos`
Registra a movimentação de saída e retorno de um objeto.

| Atributo | Tipo | Obrigatório? | Descrição / Chave |
| :--- | :--- | :--- | :--- |
| `id` | Número (INTEGER) | Sim | Chave Primária (Auto-incremental). |
| `objeto_id` | Número (INTEGER) | Sim | Chave Estrangeira -> `objetos(id)`. |
| `amigo_id` | Número (INTEGER) | Sim | Chave Estrangeira -> `amigos(id)`. |
| `data_saida` | Data (DATE) | Sim | Data de início do empréstimo. |
| `prazo_devolucao` | Data (DATE) | Sim | Data limite combinada para devolução. |
| `data_devolucao` | Data (DATE) | Não | Data real da devolução (Preenchido ao encerrar). |
| `estado_devolucao` | Texto (VARCHAR 50) | Não | Estado do item na devolução (Sem Alteração, Leve, Grave, Inutilizado). |
| `status` | Texto (VARCHAR 20) | Sim | Status (Ativo, Atrasado, Encerrado, Perdido). |

### 8.4 Tabela: `renovacoes`
Histórico de prorrogações dos empréstimos.

| Atributo | Tipo | Obrigatório? | Descrição / Chave |
| :--- | :--- | :--- | :--- |
| `id` | Número (INTEGER) | Sim | Chave Primária (Auto-incremental). |
| `emprestimo_id` | Número (INTEGER) | Sim | Chave Estrangeira -> `emprestimos(id)`. |
| `nova_data_prazo` | Data (DATE) | Sim | Nova data limite definida. |
| `data_renovacao` | Data e Hora (TIMESTAMP) | Sim | Data e hora exata do registro da renovação. |

---

## 9. Casos de borda e erros

| Situação | Comportamento Esperado | Regra |
| :--- | :--- | :--- |
| Tentar deletar um amigo com histórico de empréstimo. | O sistema bloqueia a exclusão e exibe a mensagem: "Contato possui histórico e não pode ser excluído. Deseja arquivá-lo?". | RN03 |
| Alteração no fuso horário do dispositivo do usuário. | As validações de atraso e vencimento utilizam estritamente o fuso horário oficial `America/Sao_Paulo` via servidor/API. | RN06, RN08 |
| Objeto devolvido com estragos totais (`Inutilizado`). | O empréstimo é encerrado, mas o objeto fica indisponível para novos empréstimos até que o Dono edite manualmente o estado do item. | RN13, RN14 |
| Amigo solicita exclusão de seus dados (LGPD). | O sistema executa a anonimização dos dados cadastrais do amigo, preservando a integridade das estatísticas de empréstimos anteriores. | RN04 |
| Mudança de categoria do objeto com empréstimo ativo. | A alteração da categoria é permitida, porém a nova data padrão não afeta os empréstimos que já estão em andamento. | RN05 |

---

## 10. Requisitos não funcionais

* **Desempenho:** A listagem de empréstimos e buscas no histórico deve responder em no máximo 1,5 segundo para bases com até 5.000 registros.
* **Privacidade e Segurança (LGPD):** Os dados dos contatos devem ser armazenados localmente ou de forma criptografada no banco. Deve haver a funcionalidade de anonimização sob demanda.
* **Plataforma:** Aplicativo mobile responsivo compatível com iOS e Android.
* **Design & Identidade Visual (Paleta de Cores Exclusiva):**
  A interface do aplicativo utilizará como base visual a paleta informada:
  * **Vanilla Cream (`#FFF7E6`):** Cor de fundo primária, superfícies claras e cards de leitura.
  * **Blush Petal (`#F7C8D3`):** Cor secundária para destaques suaves, tags de status `Ativo` e botões secundários.
  * **Rosewood (`#B46A72`):** Cor de acento primária para botões de ação principal, cabeçalhos, alertas e indicação de status `Atrasado`.

---

## 11. Decisões e premissas

| ID | Decisão | Alternativas Consideradas | Justificativa |
| :--- | :--- | :--- | :--- |
| **DEC01** | O Tomador não possui acesso ou conta no app. | Criar aplicativo multifuncional para Dono e Tomador com chat. | Reduz radicalmente a complexidade do app, elimina necessidade de rede/chat e foca no uso estritamente pessoal do Dono. |
| **DEC02** | Contagem de prazos em dias fixos com vencimento até 23:59:59. | Permitir escolha de hora exata de devolução (ex: 14:30). | Simplifica a experiência do usuário e evita falsos alarmes de atraso durante o horário comercial/social. |
| **DEC03** | Limite rígido de 3 renovações por empréstimo. | Renovações ilimitadas ou sem renovação. | Evita que o objeto fique emprestado indefinidamente sem que haja uma tomada de decisão (devolução ou perda). |
| **DEC04** | Anonimização dos dados do amigo em caso de solicitação LGPD. | Exclusão em cascata de todo o histórico do amigo. | Mantém o histórico do inventário do Dono consistente sem violar o direito de remoção de dados pessoais do contato. |

---

## 12. Questões em aberto

* *Nenhuma questão em aberto nesta versão.* Todas as definições de prazos, atrasos, danos, perdas e estrutura do modelo de dados foram consolidadas.
