# AppEmprestaAi

Empresta Aí  
Mirela Rodrigues - Data: 03/11/2026 | Revisão v1.2 (MVP): 05/11/2026

---

## 1. Visão
Pessoas que costumam emprestar objetos pessoais (como livros, ferramentas e jogos) para amigos e frequentemente perdem o controle desses pertences ou do prazo de devolução. O Empresta Aí é um projeto mobile conceitual focado no Dono do objeto que visa organizar e registrar empréstimos, controlar prazos e notificar sobre pendências. Com o projeto, o usuário obtém histórico transparente de empréstimos, minimiza perdas permanentes e mantém uma relação saudável e organizada com seus contatos.

---

## 2. Escopo

### 2.1 Dentro do escopo
* Cadastrar objetos com nome, categoria, foto opcional e estado de conservação inicial.
* Cadastrar amigos/contatos com nome, telefone e e-mail (opcional), além de arquivá-los quando houver histórico.
* Registrar empréstimo associando um objeto disponível a um amigo cadastrado, definindo data de saída e prazo de devolução.
* Renovar empréstimo ativo ou atrasado, alterando a data limite de devolução.
* Registrar devolução (total ou parcial) de objeto, especificando a data real de retorno e o estado de devolução do item (`Sem Alteração`, `Dano Leve`, `Dano Grave` ou `Inutilizado`).
* Registrar perda ou não devolução de objeto (status `Perdido`) e registrar a recuperação de um objeto perdido.
* Listar e filtrar empréstimos por status: Ativos, Atrasados, Encerrados e Perdidos.
* Enviar notificações para o Dono sobre prazos prestes a vencer e atrasos consolidados.
* Consultar histórico completo de movimentações por Objeto e por Amigo.

### 2.2 Fora do escopo (Limitações explícitas)
* Cobrança ou pagamento real de multas, aluguéis ou indenizações: O projeto não realiza transações financeiras.
* Marketplace público de objetos: Não há compartilhamento ou busca de objetos entre desconhecidos ou rede pública.
* Chat/mensageria entre usuários: O projeto não possui sistema interno de troca de mensagens.
* Rastreamento por geolocalização: Não há uso de GPS nem rastreamento físico de objetos ou pessoas.

---

## 3. Glossário
* **Dono**: Único usuário autenticado do projeto, proprietário dos objetos e gerenciador do acervo.
* **Tomador**: Pessoa (amigo/contato) cadastrada pelo Dono que recebe um objeto emprestado. Não possui conta nem acesso ao projeto.
* **Objeto**: Item individual ou conjunto indivisível cadastrado pelo Dono. Um conjunto é sempre uma única unidade de cadastro (RN02).
* **Empréstimo Ativo**: Registro em que o objeto foi entregue ao Tomador, o prazo limite de devolução é igual ou posterior à data atual (considerando o fuso horário America/Sao_Paulo) e a devolução ainda não foi confirmada.
* **Atrasado**: Registro em que a devolução não ocorreu e a data atual é posterior ao prazo limite de devolução (a partir das 00:00:00 do dia seguinte ao vencimento).
* **Encerrado**: Estado final do empréstimo após o registro da devolução total do objeto.
* **Perdido**: Estado final do empréstimo (e do objeto) quando o Dono declara que o objeto, ou o saldo pendente de um conjunto, não será devolvido. O Dono pode declarar a perda manualmente a qualquer momento; após 30 dias de atraso o sistema apenas exibe um atalho sugerindo a ação (RN16). Substitui qualquer outra denominação de perda (ex.: "não devolvido", "extraviado").
* **Devolução Parcial**: Devolução de apenas parte de um Objeto do tipo conjunto. Não encerra o empréstimo (RN15).
* **Saldo Pendente**: Descrição textual, registrada pelo Dono, das partes de um conjunto que ainda não foram devolvidas. Não divide o Objeto nem cria novos objetos no sistema.
* **Estado de Devolução**: Avaliação do objeto no momento da devolução. Vocabulário único e exclusivo: `Sem Alteração`, `Dano Leve`, `Dano Grave`, `Inutilizado`.
* **Renovação**: Ato de estender a data de devolução limite de um empréstimo ativo ou atrasado.
* **Arquivado**: Condição de um objeto ou amigo desativado, que permanece no histórico mas não pode ser selecionado em novos empréstimos (RN03).
* **Dias de Atraso**: Calculado estritamente como $\text{data atual} - \text{prazo}$. Pelo critério, um vencimento em 30/09/2026 avaliado em 05/11/2026 resulta em 37 dias de atraso.
* **Objeto Inutilizado**: Objeto com estado de conservação `Inutilizado`. Continua com situação cadastral "Disponível" no sistema, mas fica bloqueado e não pode ser selecionado para novos empréstimos até que o Dono edite manualmente seu estado.
* **Lembrete de Vencimento (Prevenção)**: Alerta emitido na véspera do vencimento. Caso a véspera já tenha passado (ex: empréstimos de 1 dia de duração), o lembrete de prevenção não é enviado.

---

## 4. Escopo do MVP (Foco Inicial de Implementação)
Para guiar o desenvolvimento e validar a hipótese principal do projeto com o menor esforço e maior aprendizado, o MVP restringe o escopo de implementação aos seguintes itens:

* **HU01 - Cadastrar Objeto**: Apenas os Cenários C01 a C03.
* **HU02 - Registrar Empréstimo**: Apenas os Cenários C01 a C03.
* **HU03 - Renovar Empréstimo**: Apenas os Cenários C01 a C04.
* **HU04 - Registrar Devolução**: Apenas os Cenários C01 a C05 e C07.
* **HU05 - Cadastrar e Arquivar Amigo**: Apenas os Cenários C01 a C02.
* **HU06 - Marcar Empréstimo como Perdido**: Apenas os Cenários C02 a C04.
* **HU07 - Listar e Filtrar Empréstimos**: Apenas os Cenários C01 a C03.
* **HU08 - Receber Notificações de Prazo**: Apenas o Cenário C01 (restrito exclusivamente ao aviso de véspera e ao 1º dia de atraso) e o Cenário C03.

**Itens adiados para a v1.1+:**
* HU09 (Consulta de histórico completo)
* HU10 (Recuperação de objeto perdido)
* Devolução parcial e tabela de ajustes
* Arquivamento completo e LGPD (anonimização)
* Buscas avançadas, teto de 365 dias e atalho de sugestão de perda automática após 30 dias.

---

## 5. Atores e permissões

| Ator | Pode fazer | Não pode fazer |
| :--- | :--- | :--- |
| **Dono** | • Cadastrar, editar, arquivar e remover (apenas sem histórico) objetos e amigos.<br>• Criar, renovar, encerrar e marcar perdas em empréstimos (a qualquer momento).<br>• Registrar devoluções parciais e a recuperação de objetos perdidos.<br>• Visualizar relatórios, notificações e históricos. | • Alterar registros de empréstimos já encerrados ou perdidos (a recuperação de um objeto perdido é feita por novo registro de ajuste, conforme RN17, sem alterar o empréstimo original).<br>• Excluir ou arquivar contatos/objetos vinculados a empréstimos ativos/atrasados. |
| **Tomador** | • Receber comunicações externas do Dono (fora do projeto). | • Acessar o sistema, visualizar telas ou interagir diretamente com a interface do projeto. |

---

## 6. Regras de negócio

### Gestão de Objetos e Amigos
* **RN01**: Um objeto pode ter no máximo 1 empréstimo ativo ou atrasado por vez. Somente objetos com status `Disponível` e com estado de conservação diferente de `Inutilizado` podem ser selecionados para um novo empréstimo.
* **RN02**: Um conjunto de itens agrupados (ex: "Coleção Harry Potter 7 Livros") deve ser cadastrado como um único objeto no sistema.
* **RN03**: Não é permitida a exclusão de um objeto ou amigo que possua histórico de empréstimo (ativo, atrasado, encerrado ou perdido). Nestes casos, o item ou amigo só pode ser arquivado/desativado (`status` = `Arquivado` em `objetos`; `arquivado` = verdadeiro em `amigos`). Itens arquivados permanecem visíveis no histórico, mas não podem ser selecionados em novos empréstimos. Não é permitido arquivar objeto ou amigo vinculado a empréstimo ativo ou atrasado.
* **RN04**: A remoção completa de dados de um amigo (conforme LGPD) apaga seus dados pessoais (nome, telefone, email), mantendo o histórico de empréstimo com o tomador rotulado anonimamente como `"Contato Removido"`.

### Prazos e Vigência
* **RN05**: A data de devolução padrão é definida automaticamente por categoria, podendo ser sobrescrita manualmente no momento da criação:
  * Ferramentas: 7 dias
  * Eletrônicos: 14 dias
  * Livros/Jogos: 30 dias
  * Outros: 15 dias
* **RN06**: O prazo limite de devolução vence rigorosamente às 23:59:59 (horário de Brasília - UTC-3) do dia estipulado.
* **RN07**: O prazo mínimo de um empréstimo é de 1 dia e o prazo máximo é de 365 dias, contados a partir da `data_saida`. O limite de 365 dias aplica-se ao **prazo acumulado total**, incluindo todas as renovações: o `prazo_devolucao` vigente nunca pode ultrapassar `data_saida + 365 dias`. Empréstimos sem prazo definido não são permitidos.

### Atrasos e Notificações
* **RN08**: Um empréstimo é considerado Atrasado a partir da 00:00:00 do dia subsequente ao prazo de devolução. Não há período de tolerância.
* **RN09**: O projeto emitirá alertas locais ao Dono com a seguinte frequência:
  * **Aviso de Prevenção**: 1 dia antes do vencimento, às 09:00 AM (exceto se o prazo inicial for inferior a 1 dia, caso em que a véspera não existe).
  * **Aviso de Vencimento**: No dia do vencimento, às 09:00 AM.
  * **Aviso de Atraso**: Diariamente a partir do 1º dia de atraso até o 7º dia de atraso, às 10:00 AM. A partir do 8º dia, os avisos cessam.
  * Os avisos pendentes são cancelados ao encerrar o empréstimo ou declará-lo `Perdido`, e reagendados a cada renovação.

### Renovações
* **RN10**: Apenas o Dono pode executar a renovação dentro do projeto.
* **RN11**: Cada empréstimo pode ser renovado no máximo 3 vezes.
* **RN12**: A renovação pode ser feita em empréstimos com status `Ativo` ou `Atrasado`. A nova data de devolução é o **maior valor** entre o prazo limite atual e `data_atual + dias_adicionados`, ou seja, `nova_data = MAX(prazo_devolucao_atual, data_atual + dias_adicionados)`. Assim, uma renovação feita antes do vencimento nunca encurta o prazo vigente. Se o resultado for igual ao prazo atual (a renovação não ampliaria o prazo), o sistema não a registra e não consome o contador de renovações. A nova data também deve respeitar o limite acumulado de 365 dias da RN07; caso contrário, a renovação é bloqueada.

### Devoluções, Danos e Perdas
* **RN13**: Ao registrar a devolução, o Dono deve selecionar obrigatoriamente o estado de devolução entre as opções: `Sem Alteração`, `Dano Leve`, `Dano Grave`, `Inutilizado`.
* **RN14**: O estado de devolução produz os seguintes efeitos no objeto, após o encerramento do empréstimo:
  * `Sem Alteração`: nenhuma alteração; o objeto volta para `Disponível`.
  * `Dano Leve`: o objeto volta para `Disponível` e continua elegível para novos empréstimos, com o estado de conservação inalterado. O dano é registrado no histórico do objeto e do empréstimo, e o sistema exibe um alerta informativo (não bloqueante) nos detalhes do objeto e ao selecioná-lo para um novo empréstimo.
  * `Dano Grave`: o estado de conservação do objeto é atualizado automaticamente para `Danificado`; o objeto volta para `Disponível`, com o dano registrado no histórico.
  * `Inutilizado`: o estado de conservação do objeto é atualizado automaticamente para `Inutilizado`; o objeto fica indisponível para novos empréstimos (RN01) até que o Dono edite manualmente o estado do item.
* **RN15**: Devolução parcial aplica-se apenas a objetos do tipo conjunto (RN02) e não encerra o empréstimo. O objeto continua sendo uma única unidade de cadastro: o sistema não o divide nem cria registros para as partes. O Dono registra a devolução parcial como um ajuste no histórico, descrevendo em texto o saldo pendente. Enquanto houver saldo, o empréstimo permanece `Ativo` ou `Atrasado` (com prazos, notificações e renovações seguindo as regras normais) e o objeto permanece `Emprestado`. O empréstimo só é concluído quando (a) o saldo restante é devolvido, passando a `Encerrado` com avaliação do estado de devolução (RN13), ou (b) o saldo pendente é declarado `Perdido` pelo Dono (RN16), passando o empréstimo e o objeto a `Perdido`, com as partes já devolvidas preservadas no histórico.
* **RN16**: Um empréstimo em atraso há mais de 30 dias (a partir do 31º dia de atraso) exibe um atalho sugerindo que seja declarado como `Perdido`. O atalho é apenas uma sugestão/alerta do sistema: o Dono pode declarar o empréstimo como `Perdido` manualmente a qualquer momento, em empréstimos `Ativo` ou `Atrasado`, sem aguardar os 30 dias. Ao declarar a perda, o empréstimo passa a `Perdido` e o objeto passa a `Perdido`.
* **RN17**: Caso um objeto marcado como `Perdido` seja recuperado, o Dono registra uma nova entrada de ajuste no histórico (tipo `Recuperação`), que altera o status do objeto de `Perdido` para `Disponível`, permitindo informar o estado de conservação atual. O empréstimo original permanece `Perdido` e não é reaberto nem alterado.

---

## 7. Histórias de usuário

**HU01 - Cadastrar Objeto**  
Como Dono  
Quero cadastrar um objeto com suas características iniciais  
Para mantê-lo disponível em meu inventário de empréstimos.  
* **Regras Relacionadas:** RN02  
* **Cenário 01: Cadastro com sucesso**  
  * **Dado** que estou na tela de cadastro de novo objeto,  
  * **Quando** preencho o Nome com "Furadeira de Impacto 500W", Categoria com "Ferramentas" e Estado de Conservação com "Bom",  
  * **E** confirmo a gravação,  
  * **Então** o objeto é salvo no banco de dados e exibido na lista de objetos com status "Disponível".  
* **Cenário 02: Validação de campo obrigatório**  
  * **Dado** que estou na tela de cadastro de novo objeto,  
  * **Quando** deixo o campo Nome em branco e tento salvar,  
  * **Então** o sistema exibe a mensagem de erro "O nome do objeto é obrigatório." e o registro não é finalizado.  
* **Cenário 03: Categoria ou estado de conservação não informados**  
  * **Dado** que estou na tela de cadastro com o Nome preenchido,  
  * **Когда** deixo a Categoria ou o Estado de Conservação sem seleção e confirmo,  
  * **Então** o sistema exibe a mensagem "Selecione a categoria e o estado de conservação do objeto." e o registro não é finalizado.  
* **Cenário 04: Cadastro de conjunto como objeto único**  
  * **Dado** que estou na tela de cadastro de novo objeto,  
  * **Quando** cadastro "Coleção Harry Potter 7 Livros" com Categoria "Livros" e confirmo,  
  * **Então** o sistema cria um único objeto com status "Disponível", sem gerar registros individuais para cada livro.

---

**HU02 - Registrar Empréstimo**  
Como Dono  
Quero associar um objeto disponível a um amigo especificando a data limite  
Para ter o controle de para quem o item foi entregue e quando deve voltar.  
* **Regras Relacionadas:** RN01, RN03, RN05, RN06, RN07  
* **Cenário 01: Registrar empréstimo com prazo padrão**  
  * **Dado** que tenho o objeto "Jogo Catan" (Categoria: Jogos) com status "Disponível" e o amigo "Carlos Silva" cadastrado,  
  * **Quando** crio um empréstimo para "Carlos Silva" no dia 03/11/2026 sem alterar o prazo sugerido,  
  * **Então** o sistema define a data de devolução para 03/12/2026 (30 dias), salva o empréstimo como "Ativo" e altera o status do objeto para "Emprestado".  
* **Cenário 02: Tentativa de emprestar objeto já emprestado**  
  * **Dado** que o objeto "Livro Clean Code" possui um empréstimo com status "Ativo" (ou "Atrasado") e status de objeto "Emprestado",  
  * **Quando** tento selecionar "Livro Clean Code" para criar um novo empréstimo,  
  * **Então** o sistema impede a seleção e exibe a mensagem "Este objeto já está em um empréstimo ativo."  
* **Cenário 03: Prazo fora dos limites permitidos**  
  * **Dado** que estou criando um empréstimo e sobrescrevo o prazo sugerido,  
  * **Quando** informo um prazo de 0 dia ou de mais de 365 dias,  
  * **Então** o sistema exibe a mensagem "O prazo deve ser de no mínimo 1 e no máximo 365 dias." e o empréstimo não é salvo.  
* **Cenário 04: Amigo arquivado ou objeto indisponível**  
  * **Dado** que o amigo "Ana Souza" está arquivado e o objeto "Serra Tico-Tico" está com status "Perdido", "Arquivado" ou estado de conservação "Inutilizado",  
  * **Quando** crio um novo empréstimo,  
  * **Então** "Ana Souza" não aparece na seleção de amigos e "Serra Tico-Tico" não aparece na seleção de objetos disponíveis.

---

**HU03 - Renovar Empréstimo**  
Como Dono  
Quero estender a data limite de devolução de um empréstimo  
Para conceder mais tempo ao amigo sem marcar o item como atrasado.  
* **Regras Relacionadas:** RN07, RN10, RN11, RN12  
* **Cenário 01: Renovação válida dentro do limite**  
  * **Dado** que um empréstimo possui 0 renovações registradas e prazo limite para 10/11/2026,  
  * **Quando** aciono "Renovar", informando mais 7 dias no dia 08/11/2026,  
  * **Então** a nova data limite é atualizada para 15/11/2026 (maior valor entre 10/11/2026 e 08/11/2026 + 7 dias), o contador de renovações sobe para 1 e um registro é adicionado na tabela de Renovações.  
* **Cenário 02: Limite de renovações atingido**  
  * **Dado** que um empréstimo já possui 3 renovações cadastradas,  
  * **Quando** visualizo os detalhes do empréstimo,  
  * **Então** o botão "Renovar" é exibido desabilitado e a mensagem "Limite máximo de 3 renovações atingido" é apresentada.  
* **Cenário 03: Renovação antecipada que não encurta o prazo**  
  * **Dado** que um empréstimo ativo possui prazo limite para 30/11/2026,  
  * **Quando** aciono "Renovar" no dia 08/11/2026 informando mais 7 dias (08/11/2026 + 7 = 15/11/2026),  
  * **Então** o sistema mantém o prazo em 30/11/2026, exibe a mensagem "A renovação não amplia o prazo atual. Informe uma quantidade maior de dias." e não registra a renovação nem incrementa o contador.  
* **Cenário 04: Renovação de empréstimo atrasado**  
  * **Dado** que um empréstimo está com status "Atrasado" e prazo vencido em 01/11/2026,  
  * **Quando** renovo no dia 05/11/2026 com mais 10 dias,  
  * **Então** a nova data limite passa a ser 15/11/2026, o status retorna para "Ativo" e o contador de renovações é incrementado.  
* **Cenário 05: Prazo acumulado acima de 365 dias**  
  * **Dado** que um empréstimo foi iniciado em 03/11/2026 (limite máximo acumulado de 03/11/2027) e possui prazo atual em 20/10/2027,  
  * **Quando** tento renovar no dia 21/10/2027 com mais 30 dias (nova data: 20/11/2027),  
  * **Então** o sistema bloqueia a renovação e exibe a mensagem "O prazo total do empréstimo não pode exceder 365 dias a partir da data de saída (limite: 03/11/2027)."  
* **Cenário 06: Quantidade de dias inválida**  
  * **Dado** que estou na tela de renovação,  
  * **Quando** informo 0 dia ou um valor negativo,  
  * **Então** o sistema exibe a mensagem "Informe pelo menos 1 dia." e a renovação não é registrada.

---

**HU04 - Registrar Devolução**  
Como Dono  
Quero registrar a devolução de um objeto informando seu estado final  
Para encerrar o empréstimo e atualizar o status do meu objeto.  
* **Regras Relacionadas:** RN13, RN14, RN15  
* **Cenário 01: Devolução em perfeito estado**  
  * **Dado** que o empréstimo do objeto "Martelo" está com status "Ativo",  
  * **Quando** registro a devolução informando a data atual e seleciono o estado "Sem Alteração",  
  * **Então** o status do empréstimo muda para "Encerrado", a data_devolucao é preenchida e o status do objeto "Martelo" volta para "Disponível".  
* **Cenário 02: Devolução com Dano Leve**  
  * **Dado** que o empréstimo do objeto "Furadeira de Impacto 500W" está com status "Ativo" e estado de conservação "Bom",  
  * **Когда** registro a devolução e seleciono o estado "Dano Leve",  
  * **Então** o empréstimo é "Encerrado", o objeto volta para "Disponível" mantendo o estado de conservação "Bom", o dano é registrado no histórico e o sistema exibe um alerta informativo "Dano Leve registrado na última devolução" nos detalhes do objeto e ao selecioná-lo para novo empréstimo, sem impedir a seleção.  
* **Cenário 03: Devolução com Dano Grave**  
  * **Dado** que o empréstimo do objeto "Notebook Velho" está com status "Ativo",  
  * **Когда** registro a devolução e seleciono o estado "Dano Grave",  
  * **Então** o empréstimo é "Encerrado", o estado de conservação do objeto é atualizado automaticamente para "Danificado", o objeto volta para "Disponível" e o dano é registrado no histórico.  
* **Cenário 04: Devolução com objeto Inutilizado**  
  * **Dado** que o empréstimo do objeto "Jogo Catan" está com status "Ativo",  
  * **Когда** registro a devolução e seleciono o estado "Inutilizado",  
  * **Então** o empréstimo é "Encerrado", o estado de conservação do objeto é atualizado para "Inutilizado" e o objeto deixa de aparecer na seleção de novos empréstimos até que eu edite manualmente o estado do item.  
* **Cenário 05: Estado de devolução não informado**  
  * **Dado** que estou na tela de devolução do empréstimo do objeto "Martelo",  
  * **Quando** não seleciono nenhuma opção de estado e confirmo,  
  * **Então** o sistema exibe a mensagem "Selecione o estado de devolução do objeto." e o empréstimo permanece em seu status atual.  
* **Cenário 06: Data de devolução inválida**  
  * **Dado** que estou na tela de devolução de um empréstimo com data_saida em 03/11/2026,  
  * **Когда** informo uma data de devolução anterior a 03/11/2026 ou posterior à data atual,  
  * **Então** o sistema exibe a mensagem "A data de devolução não pode ser anterior à data de saída nem posterior à data atual." e o registro não é finalizado.  
* **Cenário 07: Devolução de empréstimo atrasado**  
  * **Dado** que o empréstimo do objeto "Livro Clean Code" está com status "Atrasado",  
  * **Когда** registro a devolução com o estado "Sem Alteração",  
  * **Então** o empréstimo é "Encerrado", o objeto volta para "Disponível" e o histórico mantém a informação de que a devolução ocorreu após o prazo limite.  
* **Cenário 08: Devolução parcial de conjunto**  
  * **Dado** que o empréstimo do objeto "Coleção Harry Potter 7 Livros" está com status "Ativo",  
  * **Quando** registro uma devolução parcial informando como saldo pendente "Livros 6 e 7",  
  * **Então** o empréstimo permanece "Ativo", o objeto permanece "Emprestado", o ajuste é registrado no histórico com o saldo pendente e o prazo limite não é alterado.  
* **Cenário 09: Devolução do saldo pendente**  
  * **Dado** que o empréstimo da "Coleção Harry Potter 7 Livros" possui devolução parcial registrada,  
  * **Quando** registro a devolução do saldo restante e seleciono o estado "Sem Alteração",  
  * **Então** o empréstimo é "Encerrado" e o objeto volta para "Disponível".

---

**HU05 - Cadastrar e Arquivar Amigo**  
Como Dono  
Quero cadastrar meus amigos com seus dados de contato e arquivá-los quando não forem mais necessários  
Para associá-los aos empréstimos e manter meu histórico organizado.  
* **Regras Relacionadas:** RN03, RN04  
* **Cenário 01: Cadastro com sucesso**  
  * **Dado** que estou na tela de cadastro de novo amigo,  
  * **Quando** preencho Nome com "Carlos Silva" e Telefone com "(11) 91234-5678", deixo o e-mail em branco e confirmo,  
  * **Então** o amigo é salvo com `arquivado` = falso e passa a aparecer na seleção de amigos para novos empréstimos.  
* **Cenário 02: Campos obrigatórios não preenchidos**  
  * **Dado** que estou na tela de cadastro de novo amigo,  
  * **Quando** deixo o Nome ou o Telefone em branco e tento salvar,  
  * **Então** o sistema exibe a mensagem "Nome e telefone são obrigatórios." e o registro não é finalizado.  
* **Cenário 03: E-mail em formato inválido**  
  * **Dado** que estou na tela de cadastro com Nome e Telefone preenchidos,  
  * **Quando** informo o e-mail "carlos@" e tento salvar,  
  * **Então** o sistema exibe a mensagem "Informe um e-mail válido ou deixe o campo em branco." e o registro não é finalizado.  
* **Cenário 04: Arquivar amigo com histórico**  
  * **Dado** que o amigo "Carlos Silva" possui apenas empréstimos encerrados ou perdidos,  
  * **Quando** tento excluí-lo,  
  * **Então** o sistema bloqueia a exclusão, exibe a mensagem "Contato possui histórico e não pode ser excluído. Deseja arquivá-lo?" e, ao confirmar, marca `arquivado` = verdadeiro, mantendo o histórico visível.  
* **Cenário 05: Amigo com empréstimo ativo ou atrasado**  
  * **Dado** que o amigo "Carlos Silva" possui um empréstimo "Ativo" ou "Atrasado",  
  * **Когда** tento excluí-lo ou arquivá-lo,  
  * **Então** o sistema bloqueia a ação e exibe a mensagem "Este contato possui empréstimos em andamento."  
* **Cenário 06: Exclusão de amigo sem histórico**  
  * **Dado** que o amigo "Ana Souza" não possui nenhum empréstimo registrado,  
  * **Quando** solicito a exclusão e confirmo,  
  * **Então** o cadastro é removido definitivamente.  
* **Cenário 07: Remoção de dados pessoais (LGPD)**  
  * **Dado** que o amigo "Carlos Silva" solicitou a remoção de seus dados e possui histórico de empréstimos,  
  * **Quando** executo a anonimização,  
  * **Então** nome, telefone e e-mail são apagados e o histórico passa a exibir o tomador como "Contato Removido".

---

**HU06 - Marcar Empréstimo como Perdido**  
Como Dono  
Quero declarar um objeto como perdido quando ele não for devolvido  
Para encerrar a pendência e manter o histórico fiel à realidade.  
* **Regras Relacionadas:** RN09, RN15, RN16  
* **Cenário 01: Atalho sugerido após 30 dias de atraso**  
  * **Dado** que um empréstimo está com status "Atrasado" desde 30/09/2026 e a data atual é 05/11/2026 (37 dias de atraso pelo cálculo $\text{data atual} - \text{prazo}$),  
  * **Quando** abro os detalhes do empréstimo,  
  * **Então** o sistema exibe o alerta "Atraso superior a 30 dias" com o atalho "Declarar como Perdido".  
* **Cenário 02: Declarar perda com confirmação**  
  * **Dado** que estou nos detalhes de um empréstimo "Atrasado",  
  * **Когда** aciono "Declarar como Perdido" e confirmo a mensagem "Esta ação encerra o empréstimo como Perdido. Deseja continuar?",  
  * **Então** o empréstimo passa para "Perdido", o objeto passa para "Perdido", os avisos pendentes são cancelados e o registro entra no histórico do objeto e do amigo.  
* **Cenário 03: Declarar perda antes dos 30 dias**  
  * **Dado** que um empréstimo está "Ativo" ou "Atrasado" há menos de 30 dias,  
  * **Quando** aciono "Declarar como Perdido" pelo menu de ações do empréstimo e confirmo,  
  * **Então** o sistema aceita a declaração manual normalmente, pois o atalho de 30 dias é apenas uma sugestão.  
* **Cenário 04: Cancelar a declaração**  
  * **Dado** que aciono "Declarar como Perdido",  
  * **Quando** escolho "Cancelar" na confirmação,  
  * **Então** nenhum status é alterado.  
* **Cenário 05: Perda do saldo pendente de um conjunto**  
  * **Dado** que o empréstimo da "Coleção Harry Potter 7 Livros" possui devolução parcial com saldo "Livros 6 e 7",  
  * **Ketika/Quando** declaro o saldo pendente como "Perdido" e confirmo,  
  * **Então** o empréstimo e o objeto passam para "Perdido" e o histórico preserva a devolução parcial já registrada.  
* **Cenário 06: Empréstimo já finalizado**  
  * **Dado** que um empréstimo está "Encerrado" ou "Perdido",  
  * **Quando** abro seus detalhes,  
  * **Então** a opção "Declarar como Perdido" não é exibida.

---

**HU07 - Listar e Filtrar Empréstimos**  
Como Dono  
Quero listar e filtrar meus empréstimos por status  
Para identificar rapidamente o que está em andamento, o que está atrasado e o que já foi finalizado.  
* **Regras Relacionadas:** RN06, RN08, RN16  
* **Cenário 01: Filtrar por status**  
  * **Dado** que existem empréstimos "Ativo", "Atrasado", "Encerrado" e "Perdido",  
  * **Quando** seleciono o filtro "Atrasados",  
  * **Então** a lista exibe somente os empréstimos "Atrasado", ordenados do mais antigo para o mais recente prazo vencido.  
* **Cenário 02: Identificação de atraso sem depender de cor**  
  * **Dado** que um empréstimo está "Atrasado" há 3 dias,  
  * **Quando** visualizo a lista,  
  * **Então** o item exibe o ícone de alerta e o texto "Atrasado há 3 dias", além da cor de destaque.  
* **Cenário 03: Lista sem resultados**  
  * **Dado** que não existe nenhum empréstimo com o status selecionado,  
  * **Cuando** aplico o filtro,  
  * **Então** o sistema exibe a mensagem "Nenhum empréstimo encontrado para este filtro."  
* **Cenário 04: Busca por nome**  
  * **Dado** que estou na lista de empréstimos,  
  * **Quando** digito "Catan" no campo de busca,  
  * **Então** a lista exibe apenas empréstimos cujo objeto ou amigo contenha o termo, respondendo em até 1,5 segundo para bases com até 5.000 registros.

---

**HU08 - Receber Notificações de Prazo**  
Como Dono  
Quero receber alertas sobre vencimentos e atrasos  
Para agir antes de perder o controle do prazo de devolução.  
* **Regras Relacionadas:** RN08, RN09, RN12, RN16  
* **Cenário 01: Sequência de avisos**  
  * **Dado** que um empréstimo "Ativo" possui prazo limite em 10/11/2026 e duração maior que 1 dia,  
  * **Quando** chegam os horários programados,  
  * **Então** o projeto emite o Aviso de Prevenção em 09/11/2026 às 09:00, o Aviso de Vencimento em 10/11/2026 às 09:00 e, caso não haja devolução, o Aviso de Atraso diariamente às 10:00 de 11/11/2026 a 17/11/2026 (cessando no 8º dia). Se o empréstimo for de apenas 1 dia e a véspera já tiver passado, o aviso de prevenção é omitido.  
* **Cenário 02: Fim dos avisos de atraso**  
  * **Dado** que o empréstimo permanece atrasado,  
  * **Quando** chega o 8º dia de atraso (18/11/2026),  
  * **Então** o projeto não emite mais avisos, mas o empréstimo continua com a indicação "Atrasado" na lista.  
* **Cenário 03: Cancelamento ou reagendamento de avisos**  
  * **Dado** que existem avisos agendados para um empréstimo,  
  * **Когда** o empréstimo é devolvido, declarado "Perdido" ou renovado,  
  * **Então** os avisos anteriores são cancelados e, no caso de renovação, reagendados de acordo com a nova data limite.  
* **Cenário 04: Permissão de notificação negada**  
  * **Dado** que o Dono negou a permissão de notificações no dispositivo,  
  * **Quando** existem empréstimos próximos do vencimento ou atrasados,  
  * **Então** o projeto exibe um aviso dentro da tela inicial orientando a ativar as notificações e mantém os indicadores de prazo na lista de empréstimos.  
* **Cenário 05: Sugestão de perda após 30 dias**  
  * **Dado** que um empréstimo ultrapassou 30 dias de atraso,  
  * **Quando** abro o projeto,  
  * **Então** o empréstimo exibe o atalho "Declarar como Perdido" (RN16), sem alterar seu status automaticamente.

---

**HU09 - Consultar Histórico por Objeto e por Amigo**  
Como Dono  
Quero consultar o histórico completo de movimentações de um objeto ou de um amigo  
Para saber quem ficou com cada item, por quanto tempo e em que condições foi devolvido.  
* **Regras Relacionadas:** RN03, RN04, RN14, RN15, RN17  
* **Cenário 01: Histórico por objeto**  
  * **Dado** que o objeto "Furadeira de Impacto 500W" possui 3 empréstimos registrados,  
  * **Quando** abro o histórico do objeto,  
  * **Então** o sistema lista, do mais recente para o mais antigo, cada empréstimo com amigo, datas de saída, prazo e devolução, renovações, estado de devolução, devoluções parciais, ajustes de recuperação e alertas de "Dano Leve".  
* **Cenário 02: Histórico por amigo**  
  * **Dado** que o amigo "Carlos Silva" possui empréstimos ativos, encerrados e perdidos,  
  * **Когда** abro o histórico do amigo,  
  * **Então** o sistema lista todos os empréstimos associados a ele com seus respectivos status.  
* **Cenário 03: Amigo anonimizado**  
  * **Dado** que um amigo teve seus dados removidos conforme RN04,  
  * **Quando** consulto o histórico do objeto que ele pegou emprestado,  
  * **Então** o registro exibe "Contato Removido" no lugar do nome e do telefone.  
* **Cenário 04: Item arquivado**  
  * **Dado** que um objeto ou amigo está arquivado,  
  * **Quando** acesso seu histórico,  
  * **Então** todos os registros permanecem visíveis e consultáveis.  
* **Cenário 05: Sem movimentações**  
  * **Dado** que um objeto ou amigo recém-cadastrado não possui empréstimos,  
  * **Quando** abro o histórico,  
  * **Então** o sistema exibe a mensagem "Nenhuma movimentação registrada."

---

**HU10 - Recuperar Objeto Perdido**  
Como Dono  
Quero registrar que um objeto perdido foi recuperado  
Para voltar a emprestá-lo sem apagar o histórico da perda.  
* **Regras Relacionadas:** RN01, RN17  
* **Cenário 01: Recuperação com sucesso**  
  * **Dado** que o objeto "Martelo" está com status "Perdido",  
  * **Quando** aciono "Registrar recuperação", informo o estado de conservação atual "Desgastado" e confirmo,  
  * **Então** o sistema cria um ajuste do tipo "Recuperação" no histórico, altera o status do objeto para "Disponível" e mantém o empréstimo original como "Perdido".  
* **Cenário 02: Objeto não está perdido**  
  * **Dado** que o objeto "Jogo Catan" está com status "Disponível" ou "Emprestado",  
  * **Quando** abro seus detalhes,  
  * **Então** a opção "Registrar recuperação" não é exibida.  
* **Cenário 03: Recuperação de objeto inutilizado**  
  * **Dado** que o objeto recuperado está em condição "Inutilizado",  
  * **Quando** informo esse estado de conservação na recuperação,  
  * **Então** o objeto passa para "Disponível", mas permanece fora da seleção de novos empréstimos até que o estado seja editado (RN01).

---

## 8. Estados do empréstimo e do objeto

### 8.1 Estados do empréstimo

| Estado Atual | Evento | Próximo Estado | Regra / Condição |
| :--- | :--- | :--- | :--- |
| Nenhum | Criar Empréstimo | Ativo | Objeto deve estar com status "Disponível" e estado de conservação diferente de "Inutilizado" (RN01); amigo não pode estar arquivado (RN03). |
| Ativo | Data atual ultrapassa 23:59:59 do prazo | Atrasado | Processado automaticamente na virada do dia (RN08). |
| Ativo | Registrar Devolução Parcial | Ativo | Apenas para conjuntos; saldo pendente registrado no histórico (RN15). |
| Ativo | Registrar Devolução (total) | Encerrado | Preenchimento de `data_devolucao` e `estado_devolucao` (RN13). |
| Ativo | Solicitar Renovação | Ativo | Limite de renovações < 3 (RN11); nova data = MAX(prazo atual, data atual + dias) e dentro de 365 dias da saída (RN07, RN12). |
| Ativo | Marcar como Perdido | Perdido | Ação manual do Dono, a qualquer momento (RN16). |
| Atrasado | Registrar Devolução Parcial | Atrasado | Apenas para conjuntos; saldo pendente registrado no histórico (RN15). |
| Atrasado | Registrar Devolução (total) | Encerrado | Preenchimento de `data_devolucao` e `estado_devolucao` (RN13). |
| Atrasado | Solicitar Renovação | Ativo | Limite de renovações < 3 (RN11); nova data = MAX(prazo atual, data atual + dias) e dentro de 365 dias da saída (RN07, RN12). Status retorna para Ativo. |
| Atrasado | Marcar como Perdido | Perdido | Ação manual do Dono, a qualquer momento; o atalho de sugestão é exibido a partir de 30 dias de atraso (RN16). |
| Encerrado | (nenhum) | Encerrado | Estado final; o registro não pode ser alterado. |
| Perdido | (nenhum) | Perdido | Estado final; o registro não pode ser alterado. A recuperação do objeto é tratada no estado do objeto (8.2, RN17). |

### 8.2 Estados do objeto

| Estado Atual | Evento | Próximo Estado | Regra / Condição |
| :--- | :--- | :--- | :--- |
| Disponível | Criar Empréstimo | Emprestado | Objeto com estado de conservação diferente de "Inutilizado" (RN01). |
| Emprestado | Empréstimo Encerrado | Disponível | Aplica os efeitos do estado de devolução (RN14). Com "Inutilizado", o objeto fica "Disponível" cadastralmente mas fora de novos empréstimos até edição manual do estado. |
| Emprestado | Empréstimo declarado Perdido | Perdido | Ação manual do Dono (RN16); inclui perda do saldo pendente de conjuntos (RN15). |
| Perdido | Registrar Recuperação | Disponível | Ajuste do tipo "Recuperação" no histórico; empréstimo original permanece "Perdido" (RN17). |
| Disponível | Arquivar | Arquivado | Objeto sem empréstimo ativo ou atrasado (RN03). |
| Arquivado | Desarquivar | Disponível | Ação manual do Dono (RN03). |

---

## 9. Modelo de dados

### 9.1 Tabela: `amigos`
Guarda as informações das pessoas para quem o Dono empresta pertences.

| Atributo | Tipo | Obrigatório? | Descrição / Chave |
| :--- | :--- | :--- | :--- |
| `id` | Número (INTEGER) | Sim | Chave Primária (Auto-incremental). |
| `nome` | Texto (VARCHAR 100) | Sim | Nome completo do amigo. Substituído por "Contato Removido" na anonimização (RN04). |
| `telefone` | Texto (VARCHAR 20) | Sim | Telefone de contato. Apagado na anonimização (RN04). |
| `email` | Texto (VARCHAR 100) | Não | Endereço de e-mail opcional. |
| `arquivado` | Booleano (BOOLEAN) | Sim | Indica se o amigo está arquivado (RN03). Padrão: falso. |

### 9.2 Tabela: `objetos`
Guarda os itens cadastrados no inventário pessoal.

| Atributo | Tipo | Obrigatório? | Descrição / Chave |
| :--- | :--- | :--- | :--- |
| `id` | Número (INTEGER) | Sim | Chave Primária (Auto-incremental). |
| `nome` | Texto (VARCHAR 100) | Sim | Nome descritivo do item. |
| `categoria` | Texto (VARCHAR 50) | Sim | Categoria (Ferramentas, Livros, Eletrônicos, Jogos, Outros). |
| `foto_url` | Texto (VARCHAR 255) | Não | Caminho ou URL da foto do objeto. |
| `estado_conservacao` | Texto (VARCHAR 50) | Sim | Estado atual (Novo, Bom, Desgastado, Danificado, Inutilizado). |
| `status` | Texto (VARCHAR 20) | Sim | Situação do objeto no inventário (Disponível, Emprestado, Perdido, Arquivado). Nota: Itens "Inutilizados" mantêm status "Disponível" porém ficam bloqueados por regra de negócio (RN01). |

### 9.3 Tabela: `emprestimos`
Registra a movimentação de saída e retorno de um objeto.

| Atributo | Tipo | Obrigatório? | Descrição / Chave |
| :--- | :--- | :--- | :--- |
| `id` | Número (INTEGER) | Sim | Chave Primária (Auto-incremental). |
| `objeto_id` | Número (INTEGER) | Sim | Chave Estrangeira -> `objetos(id)`. |
| `amigo_id` | Número (INTEGER) | Sim | Chave Estrangeira -> `amigos(id)`. |
| `data_saida` | Data (DATE) | Sim | Data de início do empréstimo. |
| `prazo_devolucao` | Data (DATE) | Sim | Data limite vigente para devolução (atualizada a cada renovação; máximo de `data_saida` + 365 dias, RN07). |
| `data_devolucao` | Data (DATE) | Não | Data real da devolução total (Preenchido ao encerrar). |
| `estado_devolucao` | Texto (VARCHAR 50) | Não | Estado do item na devolução (Sem Alteração, Dano Leve, Dano Grave, Inutilizado). |
| `status` | Texto (VARCHAR 20) | Sim | Status (Ativo, Atrasado, Encerrado, Perdido). |

### 9.4 Tabela: `renovacoes`
Histórico de prorrogações dos empréstimos.

| Atributo | Tipo | Obrigatório? | Descrição / Chave |
| :--- | :--- | :--- | :--- |
| `id` | Número (INTEGER) | Sim | Chave Primária (Auto-incremental). |
| `emprestimo_id` | Número (INTEGER) | Sim | Chave Estrangeira -> `emprestimos(id)`. |
| `dias_adicionados` | Número (INTEGER) | Sim | Quantidade de dias informada pelo Dono na renovação (mínimo 1). |
| `nova_data_prazo` | Data (DATE) | Sim | Nova data limite definida: MAX(prazo atual, data atual + `dias_adicionados`) (RN12). |
| `data_renovacao` | Data e Hora (TIMESTAMP) | Sim | Data e hora exata do registro da renovação. |

### 9.5 Tabela: `ajustes_historico`
Registra eventos de ajuste que não alteram o empréstimo original: devoluções parciais (RN15) e recuperação de objetos perdidos (RN17).

| Atributo | Tipo | Obrigatório? | Descrição / Chave |
| :--- | :--- | :--- | :--- |
| `id` | Número (INTEGER) | Sim | Chave Primária (Auto-incremental). |
| `objeto_id` | Número (INTEGER) | Sim | Chave Estrangeira -> `objetos(id)`. |
| `emprestimo_id` | Número (INTEGER) | Sim | Chave Estrangeira -> `emprestimos(id)`. |
| `tipo` | Texto (VARCHAR 30) | Sim | Tipo do ajuste (Devolução Parcial, Recuperação). |
| `descricao` | Texto (VARCHAR 255) | Não | Descrição do saldo pendente (Devolução Parcial) ou observação sobre o estado do objeto recuperado (Recuperação). |
| `data_registro` | Data e Hora (TIMESTAMP) | Sim | Data e hora exata do registro do ajuste. |

---

## 10. Casos de borda e erros

| Situação | Comportamento Esperado | Regra |
| :--- | :--- | :--- |
| Tentar deletar um amigo ou objeto com histórico de empréstimo. | O sistema bloqueia a exclusão e exibe a mensagem: "Contato possui histórico e não pode ser excluído. Deseja arquivá-lo?" (para objetos, mensagem equivalente). O item arquivado permanece no histórico e deixa de aparecer na seleção de novos empréstimos. | RN03 |
| Tentar arquivar ou excluir amigo/objeto com empréstimo ativo ou atrasado. | O sistema bloqueia a ação e informa que há empréstimos em andamento. | RN03 |
| Alteração no fuso horário do dispositivo do usuário. | As validações de atraso e vencimento utilizam estritamente o fuso horário oficial `America/Sao_Paulo` via servidor/API. | RN06, RN08 |
| Objeto devolvido com `Dano Leve`. | O empréstimo é encerrado, o objeto volta para `Disponível` com o estado de conservação inalterado, e o dano é registrado no histórico com alerta informativo (não bloqueante) em novas seleções. | RN13, RN14 |
| Objeto devolvido com `Dano Grave`. | O empréstimo é encerrado, o estado de conservação passa automaticamente para `Danificado` e o objeto volta para `Disponível`, com o dano registrado no histórico. | RN13, RN14 |
| Objeto devolvido com estragos totais (`Inutilizado`). | O empréstimo é encerrado, mas o objeto fica indisponível para novos empréstimos até que o Dono edite manualmente o estado do item. | RN01, RN13, RN14 |
| Amigo solicita exclusão de seus dados (LGPD). | O sistema executa a anonimização dos dados cadastrais do amigo, preservando a integridade das estatísticas de empréstimos anteriores. | RN04 |
| Mudança de categoria do objeto com empréstimo ativo. | A alteração da categoria é permitida, porém a nova data padrão não afeta os empréstimos que já estão em andamento. | RN05 |
| Renovação feita antes do vencimento resultando em data menor ou igual ao prazo atual. | O prazo nunca é encurtado: o sistema mantém a data vigente, informa que a renovação não amplia o prazo e não consome o contador de renovações. | RN12 |
| Renovação que ultrapassaria 365 dias acumulados desde a data de saída. | O sistema bloqueia a renovação e informa a data limite máxima permitida. | RN07, RN12 |
| Empréstimo em atraso há mais de 30 dias. | O sistema exibe o atalho sugerindo declarar o item como `Perdido`, sem alterar o status automaticamente. O Dono também pode declarar a perda manualmente a qualquer momento, antes ou depois dos 30 dias. | RN16 |
| Devolução parcial de conjunto. | O empréstimo permanece `Ativo` ou `Atrasado` e o objeto permanece `Emprestado`; o saldo pendente é registrado em texto no histórico, sem dividir o objeto. | RN02, RN15 |
| Saldo pendente de conjunto declarado como perdido. | O empréstimo e o objeto passam para `Perdido`, preservando no histórico as partes já devolvidas. | RN15, RN16 |
| Objeto `Perdido` é recuperado. | O Dono registra um ajuste do tipo `Recuperação`; o objeto volta para `Disponível` e o empréstimo original permanece `Perdido`. | RN17 |
| Permissão de notificações negada no dispositivo. | O projeto exibe um aviso interno orientando a ativar as notificações e mantém os indicadores visuais de prazo nas listas. | RN09 |

---

## 11. Requisitos não funcionais
* **Desempenho**: A listagem de empréstimos e buscas no histórico deve responder em no máximo 1,5 segundo para bases com até 5.000 registros.
* **Privacidade e Segurança (LGPD)**: Os dados dos contatos devem ser armazenados localmente ou de forma criptografada no banco. Deve haver a funcionalidade de anonimização sob demanda.
* **Plataforma**: Projeto mobile responsivo compatível com iOS e Android.
* **Acessibilidade**: A interface deve atender ao padrão WCAG 2.1 nível AA: contraste mínimo de 4,5:1 para textos em relação ao fundo Vanilla Cream (`#FFF7E6`) e de 3:1 para bordas e ícones. Nenhuma informação (como status de atraso) pode ser transmitida apenas por cor (ver seção 11.2).

### 11.1 Inspiração de Design e Identidade Visual (Library Card Aesthetic)
O conceito visual e a experiência de usuário (UX/UI) do projeto "Empresta Aí" são profundamente inspirados na estética nostálgica, aconchegante e analógica de um **cartão de biblioteca tradicional vintage**.

O design deve transmitir a sensação de toque de papel antigo, carimbos e anotações manuais, traduzidos para uma interface digital elegante e limpa (*cozy productivity*):
* **Tipografia e Layout:** Uso de fontes serifadas clássicas e elegantes nos títulos (lembrando carimbos ou impressões de máquinas de escrever antigas) combinadas com fontes monoespaçadas ou sem serifa limpas para os dados tabulares. A interface imita o formato de fichas pautadas e tabelas de controle clássicas de acervos literários.
* **Paleta de Cores Coesa:** Baseada em tons terrosos suaves, cremes e rosados acinzentados (como o fundo de papel envelhecido *Vanilla Cream*, detalhes em rosa queimado/marrom *Rosewood* e acentos suaves *Blush Petal*).
* **Elementos Visuais Analógicos:** Presença visual simulada de grampos metálicos de fixação superior (`paperclips`), carimbos circulares de data ou de status, e anotações manuscritas estilizadas nos registros de empréstimo, transformando a rotina de controle em uma experiência visual charmosa e prazerosa.
* **Legibilidade acima do estilo:** Elementos decorativos (grampos, carimbos, anotações manuscritas) não podem ser a única forma de transmitir uma informação e não devem reduzir o contraste dos textos; fontes manuscritas são restritas a elementos ornamentais.

### 11.2 Paleta de Cores (WCAG AA)
A interface utiliza a paleta abaixo, com os tons ajustados para atender ao contraste mínimo de 4,5:1 (texto) em relação ao fundo Vanilla Cream (`#FFF7E6`). Razões calculadas pela fórmula de luminância relativa do WCAG 2.1.

| Papel | Cor | HEX | Contraste com `#FFF7E6` | Uso |
| :--- | :--- | :--- | :--- | :--- |
| Fundo primário | **Vanilla Cream** | `#FFF7E6` | — | Fundo das telas, superfícies claras e cards de leitura. |
| Texto principal | **Tinta Café** | `#4A2C2A` | 11,69:1 | Textos, dados tabulares e ícones sobre o fundo. |
| Acento primário | **Rosewood Profundo** (ajuste do Rosewood original) | `#8A444D` | 6,53:1 | Botões de ação principal (texto `#FFF7E6` sobre o botão: 6,53:1), cabeçalhos, textos/ícones de alerta e status `Atrasado`. |
| Secundária / destaque | **Blush Petal** | `#F7C8D3` | 1,39:1 (somente superfície) | Fundo de tags de status `Ativo` e botões secundários. Nunca usada como cor de texto. Textos sobre ela usam Tinta Café (8,40:1) e o componente recebe borda Rosewood Profundo (6,53:1). |
| Decorativa | **Rosewood Original** | `#B46A72` | 3,76:1 | Somente elementos decorativos e gráficos não textuais (ilustrações, linhas pautadas, carimbos). Reprovada para texto (mínimo 4,5:1); nunca usada em textos. |
| Perdido | **Rosewood Escuro** | `#5C2E35` | 10,38:1 | Fundo de tag `Perdido` com texto `#FFF7E6` (10,38:1). |

**Status nunca dependem apenas da cor.** Cada status é sempre apresentado com ícone e texto visíveis, além da cor:

| Status | Cor de apoio | Ícone obrigatório | Texto visível |
| :--- | :--- | :--- | :--- |
| Ativo | Fundo Blush Petal, texto Tinta Café, borda Rosewood Profundo | Relógio | "Ativo" e prazo (ex.: "Vence em 10/11") |
| Atrasado | Fundo Rosewood Profundo, texto Vanilla Cream | Triângulo de alerta | "Atrasado há N dia(s)" (e carimbo "ATRASADO" nos detalhes) |
| Encerrado | Fundo Vanilla Cream, texto e borda Tinta Café | Marca de verificação | "Encerrado" |
| Perdido | Fundo Rosewood Escuro, texto Vanilla Cream | Círculo com X | "Perdido" |
| Dano Leve / Dano Grave / Inutilizado | Texto Tinta Café, borda Rosewood Profundo | Ícone de aviso | Texto exato do estado (`Dano Leve`, `Dano Grave`, `Inutilizado`) |

---

## 12. Decisões e premissas

| ID | Decisão | Alternativas Consideradas | Justificativa |
| :--- | :--- | :--- | :--- |
| DEC01 | O Tomador não possui acesso ou conta no projeto. | Criar projeto multifuncional para Dono e Tomador com chat. | Reduz radicalmente a complexidade do projeto, elimina necessidade de rede/chat e foca no uso estritamente pessoal do Dono. |
| DEC02 | Contagem de prazos em dias fixos com vencimento até 23:59:59. | Permitir escolha de hora exata de devolução (ex: 14:30). | Simplifica a experiência do usuário e evita falsos alarmes de atraso durante o horário comercial/social. |
| DEC03 | Limite rígido de 3 renovações por empréstimo. | Renovações ilimitadas ou sem renovação. | Evita que o objeto fique emprestado indefinidamente sem que haja uma tomada de decisão (devolução ou perda). |
| DEC04 | Anonimização dos dados do amigo em caso de solicitação LGPD. | Exclusão em cascata de todo o histórico do amigo. | Mantém o histórico do inventário do Dono consistente sem violar o direito de remoção de dados pessoais do contato. |
| DEC05 | O atalho de 30 dias de atraso é apenas uma sugestão; o Dono pode declarar `Perdido` a qualquer momento. | Marcar como perdido automaticamente após 30 dias; só permitir a ação após 30 dias. | Preserva a autonomia do Dono (que sabe quando um item está realmente perdido) e evita mudanças automáticas de status sem confirmação. |
| DEC06 | Renovação usa `MAX(prazo atual, data atual + dias)` e não consome renovação quando não amplia o prazo. | Sempre somar os dias à data da renovação. | Impede que uma renovação antecipada encurte o prazo já concedido e evita desperdiçar o limite de 3 renovações. |
| DEC07 | O limite de 365 dias (RN07) vale para o prazo acumulado desde a `data_saida`, incluindo renovações. | Aplicar 365 dias a cada renovação; limite apenas no prazo inicial. | Mantém um teto objetivo e coerente com a DEC03, evitando empréstimos de duração indefinida por renovações sucessivas. |
| DEC08 | Devolução parcial é registrada como ajuste descritivo (`ajustes_historico`), sem dividir o conjunto. | Cadastrar cada item do conjunto como objeto separado; controle numérico de saldo. | Respeita o conceito de conjunto indivisível (RN02) e mantém o modelo de dados simples. |
| DEC09 | A recuperação de objeto perdido gera um ajuste no histórico e não reabre o empréstimo. | Reabrir o empréstimo `Perdido`; editar o status do empréstimo. | Mantém imutáveis os empréstimos finalizados e preserva a rastreabilidade da perda e da recuperação. |
| DEC10 | Tons da paleta ajustados para WCAG AA, com ícones e textos em todos os status. | Manter os tons originais (`#B46A72`, `#F7C8D3`) para texto e indicar status somente por cor. | Garante legibilidade (Rosewood original tem 3,76:1) e acessibilidade para pessoas com daltonismo ou baixa visão. |
