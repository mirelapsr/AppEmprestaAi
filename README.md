# Empresta Aí – Especificação Conceitual do Projeto

Empresta Aí  
Mirela Rodrigues - Data: 03/11/2026 | Revisão v1.2: 05/11/2026 


---

## 1. Visão
Pessoas que costumam emprestar objetos pessoais (como livros, ferramentas e jogos) para amigos e frequentemente perdem o controle desses pertences ou do prazo de devolução. O Empresta Aí é um projeto de produto focado no Dono do objeto, com o propósito de organizar e registrar empréstimos, controlar prazos e lembrar sobre pendências. Com a solução, o usuário obtém histórico transparente de empréstimos, minimiza perdas permanentes e mantém uma relação saudável e organizada com seus contatos.

---

## 2. Escopo

### 2.1 Dentro do escopo
* Cadastrar objetos com nome, categoria, foto opcional e estado de conservação inicial.
* Cadastrar amigos/contatos com nome, telefone e e-mail (opcional), além de arquivá-los quando houver histórico.
* Registrar empréstimo associando um objeto disponível a um amigo cadastrado, definindo data de saída e prazo de devolução.
* Renovar empréstimo ativo ou atrasado, alterando a data limite de devolução.
* Registrar devolução (total ou parcial) de objeto, especificando a data real de retorno e o estado de devolução do item (`Sem Alteração`, `Dano Leve`, `Dano Grave` ou `Inutilizado`).
* Registrar perda ou não devolução de objeto (situação `Perdido`) e registrar a recuperação de um objeto perdido.
* Listar e filtrar empréstimos por situação: Ativos, Atrasados, Encerrados e Perdidos.
* Emitir lembretes ao Dono sobre prazos prestes a vencer e atrasos consolidados.
* Consultar histórico completo de movimentações por Objeto e por Amigo.

### 2.2 Fora do escopo (Limitações explícitas)
* Cobrança ou pagamento real de multas, aluguéis ou indenizações: A solução não realiza transações financeiras.
* Marketplace público de objetos: Não há compartilhamento ou busca de objetos entre desconhecidos ou rede pública.
* Comunicação entre as partes: A solução não prevê canal próprio de troca de mensagens entre Dono e Tomador.
* Rastreamento por geolocalização: Não há monitoramento de localização nem rastreamento físico de objetos ou pessoas.

---

## 3. Glossário
* **Dono**: Único usuário da solução, proprietário dos objetos e gerenciador do acervo.
* **Tomador**: Pessoa (amigo/contato) cadastrada pelo Dono que recebe um objeto emprestado. Não possui conta nem participa diretamente do controle; o relacionamento com ele ocorre fora da solução.
* **Objeto**: Item individual ou conjunto indivisível cadastrado pelo Dono. Um conjunto é sempre uma única unidade de cadastro (RN02).
* **Empréstimo Ativo**: Registro em que o objeto foi entregue ao Tomador, o prazo limite de devolução é igual ou posterior à data atual (considerando o fuso horário America/Sao_Paulo) e a devolução ainda não foi confirmada.
* **Atrasado**: Registro em que a devolução não ocorreu e a data atual é posterior ao prazo limite de devolução (a partir das 00:00:00 do dia seguinte ao vencimento).
* **Encerrado**: Situação final do empréstimo após o registro da devolução total do objeto.
* **Perdido**: Situação final do empréstimo (e do objeto) quando o Dono declara que o objeto, ou o saldo pendente de um conjunto, não será devolvido. O Dono pode declarar a perda manualmente a qualquer momento; após 30 dias de atraso a solução apenas sugere a ação (RN16). Substitui qualquer outra denominação de perda (ex.: "não devolvido", "extraviado").
* **Devolução Parcial**: Devolução de apenas parte de um Objeto do tipo conjunto. Não encerra o empréstimo (RN15).
* **Saldo Pendente**: Descrição textual, registrada pelo Dono, das partes de um conjunto que ainda não foram devolvidas. Não divide o Objeto nem cria novos objetos no controle.
* **Estado de Devolução**: Avaliação do objeto no momento da devolução. Vocabulário único e exclusivo: `Sem Alteração`, `Dano Leve`, `Dano Grave`, `Inutilizado`.
* **Renovação**: Ato de estender a data de devolução limite de um empréstimo ativo ou atrasado.
* **Arquivado**: Condição de um objeto ou amigo desativado, que permanece no histórico mas não pode ser escolhido em novos empréstimos (RN03).

---

## 4. Atores e permissões

| Ator | Pode fazer | Não pode fazer |
| :--- | :--- | :--- |
| **Dono** | • Cadastrar, editar, arquivar e remover (apenas sem histórico) objetos e amigos.<br>• Criar, renovar, encerrar e marcar perdas em empréstimos (a qualquer momento).<br>• Registrar devoluções parciais e a recuperação de objetos perdidos.<br>• Consultar históricos, lembretes e listagens de empréstimos. | • Alterar registros de empréstimos já encerrados ou perdidos (a recuperação de um objeto perdido é feita por novo registro de ajuste, conforme RN17, sem alterar o empréstimo original).<br>• Excluir ou arquivar contatos/objetos vinculados a empréstimos ativos/atrasados. |
| **Tomador** | • Receber comunicações externas do Dono (fora da solução). | • Acessar os registros ou participar diretamente do controle de empréstimos. |

---

## 5. Regras conceituais de negócio

### Gestão de Objetos e Amigos
* **RN01**: Um objeto pode ter no máximo 1 empréstimo ativo ou atrasado por vez. Somente objetos com situação `Disponível` e com estado de conservação diferente de `Inutilizado` podem ser escolhidos para um novo empréstimo.
* **RN02**: Um conjunto de itens agrupados (ex: "Coleção Harry Potter 7 Livros") deve ser cadastrado como um único objeto.
* **RN03**: Não é permitida a exclusão de um objeto ou amigo que possua histórico de empréstimo (ativo, atrasado, encerrado ou perdido). Nestes casos, o item ou amigo só pode ser arquivado/desativado (objeto com situação `Arquivado`; amigo com indicação de arquivamento). Itens arquivados permanecem visíveis no histórico, mas não podem ser escolhidos em novos empréstimos. Não é permitido arquivar objeto ou amigo vinculado a empréstimo ativo ou atrasado.
* **RN04**: A remoção completa de dados de um amigo (conforme LGPD) apaga seus dados pessoais (nome, telefone, e-mail), mantendo o histórico de empréstimo com o tomador rotulado anonimamente como `"Contato Removido"`.

### Prazos e Vigência
* **RN05**: A data de devolução padrão é definida automaticamente por categoria, podendo ser sobrescrita manualmente no momento da criação:
  * Ferramentas: 7 dias
  * Eletrônicos: 14 dias
  * Livros/Jogos: 30 dias
  * Outros: 15 dias
* **RN06**: O prazo limite de devolução vence rigorosamente às 23:59:59 (horário de Brasília - UTC-3) do dia estipulado.
* **RN07**: O prazo mínimo de um empréstimo é de 1 dia e o prazo máximo é de 365 dias, contados a partir da data de saída. O limite de 365 dias aplica-se ao **prazo acumulado total**, incluindo todas as renovações: o prazo de devolução vigente nunca pode ultrapassar a data de saída acrescida de 365 dias. Empréstimos sem prazo definido não são permitidos.

### Atrasos e Lembretes
* **RN08**: Um empréstimo é considerado Atrasado a partir da 00:00:00 do dia subsequente ao prazo de devolução. Não há período de tolerância.
* **RN09**: A solução emitirá lembretes ao Dono com a seguinte frequência:
  * **Lembrete de Prevenção**: 1 dia antes do vencimento, às 09:00.
  * **Lembrete de Vencimento**: No dia do vencimento, às 09:00.
  * **Lembrete de Atraso**: Diariamente a partir do 1º dia de atraso até o 7º dia de atraso, às 10:00. A partir do 8º dia, os lembretes cessam.
  * Os lembretes pendentes são cancelados ao encerrar o empréstimo ou declará-lo `Perdido`, e reprogramados a cada renovação.

### Renovações
* **RN10**: Apenas o Dono pode executar a renovação.
* **RN11**: Cada empréstimo pode ser renovado no máximo 3 vezes.
* **RN12**: A renovação pode ser feita em empréstimos com situação `Ativo` ou `Atrasado`. A nova data de devolução é o **maior valor** entre o prazo limite atual e a data da renovação acrescida dos dias adicionados, ou seja, `nova data = MAX(prazo de devolução atual, data atual + dias adicionados)`. Assim, uma renovação feita antes do vencimento nunca encurta o prazo vigente. Se o resultado for igual ao prazo atual (a renovação não ampliaria o prazo), a solução não a registra e não consome o contador de renovações. A nova data também deve respeitar o limite acumulado de 365 dias da RN07; caso contrário, a renovação é bloqueada.

### Devoluções, Danos e Perdas
* **RN13**: Ao registrar a devolução, o Dono deve selecionar obrigatoriamente o estado de devolução entre as opções: `Sem Alteração`, `Dano Leve`, `Dano Grave`, `Inutilizado`.
* **RN14**: O estado de devolução produz os seguintes efeitos no objeto, após o encerramento do empréstimo:
  * `Sem Alteração`: nenhuma alteração; o objeto volta para `Disponível`.
  * `Dano Leve`: o objeto volta para `Disponível` e continua elegível para novos empréstimos, com o estado de conservação inalterado. O dano é registrado no histórico do objeto e do empréstimo, e a solução sinaliza um alerta informativo (não bloqueante) sempre que o objeto for consultado ou escolhido para um novo empréstimo.
  * `Dano Grave`: o estado de conservação do objeto é atualizado automaticamente para `Danificado`; o objeto volta para `Disponível`, com o dano registrado no histórico.
  * `Inutilizado`: o estado de conservação do objeto é atualizado automaticamente para `Inutilizado`; o objeto fica indisponível para novos empréstimos (RN01) até que o Dono edite manualmente o estado do item.
* **RN15**: Devolução parcial aplica-se apenas a objetos do tipo conjunto (RN02) e não encerra o empréstimo. O objeto continua sendo uma única unidade de cadastro: a solução não o divide nem cria registros para as partes. O Dono registra a devolução parcial como um ajuste no histórico, descrevendo em texto o saldo pendente. Enquanto houver saldo, o empréstimo permanece `Ativo` ou `Atrasado` (com prazos, lembretes e renovações seguindo as regras normais) e o objeto permanece `Emprestado`. O empréstimo só é concluído quando (a) o saldo restante é devolvido, passando a `Encerrado` com avaliação do estado de devolução (RN13), ou (b) o saldo pendente é declarado `Perdido` pelo Dono (RN16), passando o empréstimo e o objeto a `Perdido`, com as partes já devolvidas preservadas no histórico.
* **RN16**: Um empréstimo em atraso há mais de 30 dias (a partir do 31º dia de atraso) recebe uma sugestão para ser declarado como `Perdido`. A sugestão é apenas um alerta da solução: o Dono pode declarar o empréstimo como `Perdido` manualmente a qualquer momento, em empréstimos `Ativo` ou `Atrasado`, sem aguardar os 30 dias. Ao declarar a perda, o empréstimo passa a `Perdido` e o objeto passa a `Perdido`.
* **RN17**: Caso um objeto marcado como `Perdido` seja recuperado, o Dono registra uma nova entrada de ajuste no histórico (tipo `Recuperação`), que altera a situação do objeto de `Perdido` para `Disponível`, permitindo informar o estado de conservação atual. O empréstimo original permanece `Perdido` e não é reaberto nem alterado.

---

## 6. Histórias de usuário

**HU01 - Cadastrar Objeto**  
Como Dono  
Quero cadastrar um objeto com suas características iniciais  
Para mantê-lo disponível em meu inventário de empréstimos.  
* **Regras Relacionadas:** RN02  
* **Cenário 01: Cadastro com sucesso**  
  * **Dado** que estou cadastrando um novo objeto,  
  * **Quando** informo o Nome "Furadeira de Impacto 500W", a Categoria "Ferramentas" e o Estado de Conservação "Bom",  
  * **E** confirmo o cadastro,  
  * **Então** o objeto é registrado no inventário com situação "Disponível".  
* **Cenário 02: Validação de informação obrigatória**  
  * **Dado** que estou cadastrando um novo objeto,  
  * **Quando** deixo o Nome em branco e tento confirmar o cadastro,  
  * **Então** a solução informa "O nome do objeto é obrigatório." e o cadastro não é concluído.  
* **Cenário 03: Categoria ou estado de conservação não informados**  
  * **Dado** que estou cadastrando um novo objeto com o Nome preenchido,  
  * **Quando** deixo a Categoria ou o Estado de Conservação sem seleção e tento confirmar o cadastro,  
  * **Então** a solução informa "Selecione a categoria e o estado de conservação do objeto." e o cadastro não é concluído.  
* **Cenário 04: Cadastro de conjunto como objeto único**  
  * **Dado** que estou cadastrando um novo objeto,  
  * **Quando** registro "Coleção Harry Potter 7 Livros" com Categoria "Livros" e confirmo o cadastro,  
  * **Então** é criado um único objeto com situação "Disponível", sem registros individuais para cada livro.

---

**HU02 - Registrar Empréstimo**  
Como Dono  
Quero associar um objeto disponível a um amigo especificando a data limite  
Para ter o controle de para quem o item foi entregue e quando deve voltar.  
* **Regras Relacionadas:** RN01, RN03, RN05, RN06, RN07  
* **Cenário 01: Registrar empréstimo com prazo padrão**  
  * **Dado** que tenho o objeto "Jogo Catan" (Categoria: Jogos) com situação "Disponível" e o amigo "Carlos Silva" cadastrado,  
  * **Quando** registro um empréstimo para "Carlos Silva" no dia 03/11/2026 sem alterar o prazo sugerido,  
  * **Então** a solução define a data de devolução para 03/12/2026 (30 dias), registra o empréstimo como "Ativo" e altera a situação do objeto para "Emprestado".  
* **Cenário 02: Tentativa de emprestar objeto já emprestado**  
  * **Dado** que o objeto "Livro Clean Code" possui um empréstimo com situação "Ativo" (ou "Atrasado") e situação de objeto "Emprestado",  
  * **Quando** tento escolher "Livro Clean Code" para um novo empréstimo,  
  * **Então** a solução impede a escolha e informa "Este objeto já está em um empréstimo ativo."  
* **Cenário 03: Prazo fora dos limites permitidos**  
  * **Dado** que estou registrando um empréstimo e substituo o prazo sugerido,  
  * **Quando** informo um prazo de 0 dia ou de mais de 365 dias,  
  * **Então** a solução informa "O prazo deve ser de no mínimo 1 e no máximo 365 dias." e o empréstimo não é registrado.  
* **Cenário 04: Amigo arquivado ou objeto indisponível**  
  * **Dado** que o amigo "Ana Souza" está arquivado e o objeto "Serra Tico-Tico" está com situação "Perdido" ou "Arquivado", ou com estado de conservação "Inutilizado",  
  * **Quando** registro um novo empréstimo,  
  * **Então** "Ana Souza" não é oferecida entre os amigos possíveis e "Serra Tico-Tico" não é oferecido entre os objetos disponíveis.

---

**HU03 - Renovar Empréstimo**  
Como Dono  
Quero estender a data limite de devolução de um empréstimo  
Para conceder mais tempo ao amigo sem marcar o item como atrasado.  
* **Regras Relacionadas:** RN07, RN10, RN11, RN12  
* **Cenário 01: Renovação válida dentro do limite**  
  * **Dado** que um empréstimo possui 0 renovações registradas e prazo limite para 10/11/2026,  
  * **Quando** renovo o empréstimo, informando mais 7 dias no dia 08/11/2026,  
  * **Então** a nova data limite passa a ser 15/11/2026 (maior valor entre 10/11/2026 e 08/11/2026 + 7 dias), o contador de renovações sobe para 1 e a renovação é adicionada ao histórico de renovações.  
* **Cenário 02: Limite de renovações atingido**  
  * **Dado** que um empréstimo já possui 3 renovações registradas,  
  * **Quando** consulto os detalhes do empréstimo,  
  * **Então** a renovação aparece como indisponível e a solução informa "Limite máximo de 3 renovações atingido".  
* **Cenário 03: Renovação antecipada que não encurta o prazo**  
  * **Dado** que um empréstimo ativo possui prazo limite para 30/11/2026,  
  * **Quando** renovo no dia 08/11/2026 informando mais 7 dias (08/11/2026 + 7 = 15/11/2026),  
  * **Então** o prazo se mantém em 30/11/2026, a solução informa "A renovação não amplia o prazo atual. Informe uma quantidade maior de dias." e não registra a renovação nem incrementa o contador.  
* **Cenário 04: Renovação de empréstimo atrasado**  
  * **Dado** que um empréstimo está com situação "Atrasado" e prazo vencido em 01/11/2026,  
  * **Quando** renovo no dia 05/11/2026 com mais 10 dias,  
  * **Então** a nova data limite passa a ser 15/11/2026, a situação retorna para "Ativo" e o contador de renovações é incrementado.  
* **Cenário 05: Prazo acumulado acima de 365 dias**  
  * **Dado** que um empréstimo foi iniciado em 03/11/2026 (limite máximo acumulado de 03/11/2027) e possui prazo atual em 20/10/2027,  
  * **Quando** tento renovar no dia 21/10/2027 com mais 30 dias (nova data: 20/11/2027),  
  * **Então** a renovação é bloqueada e a solução informa "O prazo total do empréstimo não pode exceder 365 dias a partir da data de saída (limite: 03/11/2027)."  
* **Cenário 06: Quantidade de dias inválida**  
  * **Dado** que estou renovando um empréstimo,  
  * **Quando** informo 0 dia ou um valor negativo,  
  * **Então** a solução informa "Informe pelo menos 1 dia." e a renovação não é registrada.

---

**HU04 - Registrar Devolução**  
Como Dono  
Quero registrar a devolução de um objeto informando seu estado final  
Para encerrar o empréstimo e atualizar a situação do meu objeto.  
* **Regras Relacionadas:** RN13, RN14, RN15  
* **Cenário 01: Devolução em perfeito estado**  
  * **Dado** que o empréstimo do objeto "Martelo" está com situação "Ativo",  
  * **Quando** registro a devolução informando a data atual e seleciono o estado "Sem Alteração",  
  * **Então** o empréstimo passa para "Encerrado", a data de devolução é registrada e a situação do objeto "Martelo" volta para "Disponível".  
* **Cenário 02: Devolução com Dano Leve**  
  * **Dado** que o empréstimo do objeto "Furadeira de Impacto 500W" está com situação "Ativo" e estado de conservação "Bom",  
  * **Quando** registro a devolução e seleciono o estado "Dano Leve",  
  * **Então** o empréstimo é "Encerrado", o objeto volta para "Disponível" mantendo o estado de conservação "Bom", o dano é registrado no histórico e a solução sinaliza o alerta informativo "Dano Leve registrado na última devolução" ao consultar o objeto ou escolhê-lo para novo empréstimo, sem impedir a escolha.  
* **Cenário 03: Devolução com Dano Grave**  
  * **Dado** que o empréstimo do objeto "Notebook Velho" está com situação "Ativo",  
  * **Quando** registro a devolução e seleciono o estado "Dano Grave",  
  * **Então** o empréstimo é "Encerrado", o estado de conservação do objeto é atualizado automaticamente para "Danificado", o objeto volta para "Disponível" e o dano é registrado no histórico.  
* **Cenário 04: Devolução com objeto Inutilizado**  
  * **Dado** que o empréstimo do objeto "Jogo Catan" está com situação "Ativo",  
  * **Quando** registro a devolução e seleciono o estado "Inutilizado",  
  * **Então** o empréstimo é "Encerrado", o estado de conservação do objeto é atualizado para "Inutilizado" e o objeto deixa de ser oferecido para novos empréstimos até que eu edite manualmente o estado do item.  
* **Cenário 05: Estado de devolução não informado**  
  * **Dado** que estou registrando a devolução do empréstimo do objeto "Martelo",  
  * **Quando** não seleciono nenhum estado de devolução e tento confirmar,  
  * **Então** a solução informa "Selecione o estado de devolução do objeto." e o empréstimo permanece em sua situação atual.  
* **Cenário 06: Data de devolução inválida**  
  * **Dado** que estou registrando a devolução de um empréstimo com data de saída em 03/11/2026,  
  * **Quando** informo uma data de devolução anterior a 03/11/2026 ou posterior à data atual,  
  * **Então** a solução informa "A data de devolução não pode ser anterior à data de saída nem posterior à data atual." e o registro não é concluído.  
* **Cenário 07: Devolução de empréstimo atrasado**  
  * **Dado** que o empréstimo do objeto "Livro Clean Code" está com situação "Atrasado",  
  * **Quando** registro a devolução com o estado "Sem Alteração",  
  * **Então** o empréstimo é "Encerrado", o objeto volta para "Disponível" e o histórico mantém a informação de que a devolução ocorreu após o prazo limite.  
* **Cenário 08: Devolução parcial de conjunto**  
  * **Dado** que o empréstimo do objeto "Coleção Harry Potter 7 Livros" está com situação "Ativo",  
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
  * **Dado** que estou cadastrando um novo amigo,  
  * **Quando** informo o Nome "Carlos Silva" e o Telefone "(11) 91234-5678", deixo o e-mail em branco e confirmo o cadastro,  
  * **Então** o amigo é registrado sem arquivamento e passa a ser oferecido entre os amigos possíveis para novos empréstimos.  
* **Cenário 02: Informações obrigatórias não preenchidas**  
  * **Dado** que estou cadastrando um novo amigo,  
  * **Quando** deixo o Nome ou o Telefone em branco e tento confirmar o cadastro,  
  * **Então** a solução informa "Nome e telefone são obrigatórios." e o cadastro não é concluído.  
* **Cenário 03: E-mail em formato inválido**  
  * **Dado** que estou cadastrando um novo amigo com Nome e Telefone preenchidos,  
  * **Quando** informo o e-mail "carlos@" e tento confirmar o cadastro,  
  * **Então** a solução informa "Informe um e-mail válido ou deixe a informação em branco." e o cadastro não é concluído.  
* **Cenário 04: Arquivar amigo com histórico**  
  * **Dado** que o amigo "Carlos Silva" possui apenas empréstimos encerrados ou perdidos,  
  * **Quando** tento excluí-lo,  
  * **Então** a exclusão é bloqueada, a solução informa "Contato possui histórico e não pode ser excluído. Deseja arquivá-lo?" e, ao confirmar, o amigo é arquivado, mantendo o histórico visível.  
* **Cenário 05: Amigo com empréstimo ativo ou atrasado**  
  * **Dado** que o amigo "Carlos Silva" possui um empréstimo "Ativo" ou "Atrasado",  
  * **Quando** tento excluí-lo ou arquivá-lo,  
  * **Então** a ação é bloqueada e a solução informa "Este contato possui empréstimos em andamento."  
* **Cenário 06: Exclusão de amigo sem histórico**  
  * **Dado** que o amigo "Ana Souza" não possui nenhum empréstimo registrado,  
  * **Quando** solicito a exclusão e confirmo,  
  * **Então** o cadastro é removido definitivamente.  
* **Cenário 07: Remoção de dados pessoais (LGPD)**  
  * **Dado** que o amigo "Carlos Silva" solicitou a remoção de seus dados e possui histórico de empréstimos,  
  * **Quando** executo a anonimização,  
  * **Então** nome, telefone e e-mail são apagados e o histórico passa a identificar o tomador como "Contato Removido".

---

**HU06 - Marcar Empréstimo como Perdido**  
Como Dono  
Quero declarar um objeto como perdido quando ele não for devolvido  
Para encerrar a pendência e manter o histórico fiel à realidade.  
* **Regras Relacionadas:** RN09, RN15, RN16  
* **Cenário 01: Sugestão após 30 dias de atraso**  
  * **Dado** que um empréstimo está com situação "Atrasado" desde 30/09/2026 e a data atual é 05/11/2026 (36 dias de atraso),  
  * **Quando** consulto os detalhes do empréstimo,  
  * **Então** a solução sinaliza o alerta "Atraso superior a 30 dias" com a sugestão "Declarar como Perdido".  
* **Cenário 02: Declarar perda com confirmação**  
  * **Dado** que estou consultando um empréstimo "Atrasado",  
  * **Quando** opto por "Declarar como Perdido" e confirmo a pergunta "Esta ação encerra o empréstimo como Perdido. Deseja continuar?",  
  * **Então** o empréstimo passa para "Perdido", o objeto passa para "Perdido", os lembretes pendentes são cancelados e o registro entra no histórico do objeto e do amigo.  
* **Cenário 03: Declarar perda antes dos 30 dias**  
  * **Dado** que um empréstimo está "Ativo" ou "Atrasado" há menos de 30 dias,  
  * **Quando** opto por "Declarar como Perdido" e confirmo,  
  * **Então** a declaração manual é aceita normalmente, pois a sugestão de 30 dias é apenas um alerta.  
* **Cenário 04: Cancelar a declaração**  
  * **Dado** que optei por "Declarar como Perdido",  
  * **Quando** escolho "Cancelar" na confirmação,  
  * **Então** nenhuma situação é alterada.  
* **Cenário 05: Perda do saldo pendente de um conjunto**  
  * **Dado** que o empréstimo da "Coleção Harry Potter 7 Livros" possui devolução parcial com saldo "Livros 6 e 7",  
  * **Quando** declaro o saldo pendente como "Perdido" e confirmo,  
  * **Então** o empréstimo e o objeto passam para "Perdido" e o histórico preserva a devolução parcial já registrada.  
* **Cenário 06: Empréstimo já finalizado**  
  * **Dado** que um empréstimo está "Encerrado" ou "Perdido",  
  * **Quando** consulto seus detalhes,  
  * **Então** a opção "Declarar como Perdido" não é oferecida.

---

**HU07 - Listar e Filtrar Empréstimos**  
Como Dono  
Quero listar e filtrar meus empréstimos por situação  
Para identificar rapidamente o que está em andamento, o que está atrasado e o que já foi finalizado.  
* **Regras Relacionadas:** RN06, RN08, RN16  
* **Cenário 01: Filtrar por situação**  
  * **Dado** que existem empréstimos "Ativo", "Atrasado", "Encerrado" e "Perdido",  
  * **Quando** filtro por "Atrasados",  
  * **Então** a listagem apresenta somente os empréstimos "Atrasado", ordenados do prazo vencido mais antigo para o mais recente.  
* **Cenário 02: Identificação de atraso sem depender de cor**  
  * **Dado** que um empréstimo está "Atrasado" há 3 dias,  
  * **Quando** consulto a listagem,  
  * **Então** o item apresenta o ícone de alerta e o texto "Atrasado há 3 dias", além da cor de destaque.  
* **Cenário 03: Listagem sem resultados**  
  * **Dado** que não existe nenhum empréstimo com a situação selecionada,  
  * **Quando** aplico o filtro,  
  * **Então** a solução informa "Nenhum empréstimo encontrado para este filtro."  
* **Cenário 04: Busca por nome**  
  * **Dado** que estou consultando a listagem de empréstimos,  
  * **Quando** busco pelo termo "Catan",  
  * **Então** a listagem apresenta apenas empréstimos cujo objeto ou amigo contenha o termo, respondendo em até 1,5 segundo para bases com até 5.000 registros.

---

**HU08 - Receber Lembretes de Prazo**  
Como Dono  
Quero receber lembretes sobre vencimentos e atrasos  
Para agir antes de perder o controle do prazo de devolução.  
* **Regras Relacionadas:** RN08, RN09, RN12, RN16  
* **Cenário 01: Sequência de lembretes**  
  * **Dado** que um empréstimo "Ativo" possui prazo limite em 10/11/2026,  
  * **Quando** chegam os horários programados,  
  * **Então** a solução emite o Lembrete de Prevenção em 09/11/2026 às 09:00, o Lembrete de Vencimento em 10/11/2026 às 09:00 e, caso não haja devolução, o Lembrete de Atraso diariamente às 10:00 de 11/11/2026 a 17/11/2026.  
* **Cenário 02: Fim dos lembretes de atraso**  
  * **Dado** que o empréstimo permanece atrasado,  
  * **Quando** chega o 8º dia de atraso (18/11/2026),  
  * **Então** não são emitidos novos lembretes, mas o empréstimo continua identificado como "Atrasado" nas consultas.  
* **Cenário 03: Cancelamento ou reprogramação de lembretes**  
  * **Dado** que existem lembretes programados para um empréstimo,  
  * **Quando** o empréstimo é devolvido, declarado "Perdido" ou renovado,  
  * **Então** os lembretes anteriores são cancelados e, no caso de renovação, reprogramados de acordo com a nova data limite.  
* **Cenário 04: Dono sem canal de lembretes disponível**  
  * **Dado** que o Dono não habilitou ou desativou o recebimento de lembretes,  
  * **Quando** existem empréstimos próximos do vencimento ou atrasados,  
  * **Então** a solução orienta o Dono a reativar os lembretes e mantém os indicadores de prazo visíveis na listagem de empréstimos.  
* **Cenário 05: Sugestão de perda após 30 dias**  
  * **Dado** que um empréstimo ultrapassou 30 dias de atraso,  
  * **Quando** consulto o empréstimo,  
  * **Então** a solução sinaliza a sugestão "Declarar como Perdido" (RN16), sem alterar sua situação automaticamente.

---

**HU09 - Consultar Histórico por Objeto e por Amigo**  
Como Dono  
Quero consultar o histórico completo de movimentações de um objeto ou de um amigo  
Para saber quem ficou com cada item, por quanto tempo e em que condições foi devolvido.  
* **Regras Relacionadas:** RN03, RN04, RN14, RN15, RN17  
* **Cenário 01: Histórico por objeto**  
  * **Dado** que o objeto "Furadeira de Impacto 500W" possui 3 empréstimos registrados,  
  * **Quando** consulto o histórico do objeto,  
  * **Então** a solução lista, do mais recente para o mais antigo, cada empréstimo com amigo, datas de saída, prazo e devolução, renovações, estado de devolução, devoluções parciais, ajustes de recuperação e alertas de "Dano Leve".  
* **Cenário 02: Histórico por amigo**  
  * **Dado** que o amigo "Carlos Silva" possui empréstimos ativos, encerrados e perdidos,  
  * **Quando** consulto o histórico do amigo,  
  * **Então** a solução lista todos os empréstimos associados a ele com suas respectivas situações.  
* **Cenário 03: Amigo anonimizado**  
  * **Dado** que um amigo teve seus dados removidos conforme RN04,  
  * **Quando** consulto o histórico do objeto que ele pegou emprestado,  
  * **Então** o registro apresenta "Contato Removido" no lugar do nome e do telefone.  
* **Cenário 04: Item arquivado**  
  * **Dado** que um objeto ou amigo está arquivado,  
  * **Quando** consulto seu histórico,  
  * **Então** todos os registros permanecem visíveis e consultáveis.  
* **Cenário 05: Sem movimentações**  
  * **Dado** que um objeto ou amigo recém-cadastrado não possui empréstimos,  
  * **Quando** consulto o histórico,  
  * **Então** a solução informa "Nenhuma movimentação registrada."

---

**HU10 - Recuperar Objeto Perdido**  
Como Dono  
Quero registrar que um objeto perdido foi recuperado  
Para voltar a emprestá-lo sem apagar o histórico da perda.  
* **Regras Relacionadas:** RN01, RN17  
* **Cenário 01: Recuperação com sucesso**  
  * **Dado** que o objeto "Martelo" está com situação "Perdido",  
  * **Quando** registro a recuperação, informo o estado de conservação atual "Desgastado" e confirmo,  
  * **Então** é criado um ajuste do tipo "Recuperação" no histórico, a situação do objeto passa para "Disponível" e o empréstimo original permanece "Perdido".  
* **Cenário 02: Objeto não está perdido**  
  * **Dado** que o objeto "Jogo Catan" está com situação "Disponível" ou "Emprestado",  
  * **Quando** consulto seus detalhes,  
  * **Então** a opção "Registrar recuperação" não é oferecida.  
* **Cenário 03: Recuperação de objeto inutilizado**  
  * **Dado** que o objeto recuperado está em condição "Inutilizado",  
  * **Quando** informo esse estado de conservação na recuperação,  
  * **Então** o objeto passa para "Disponível", mas permanece fora das opções de novos empréstimos até que o estado seja editado (RN01).

---

## 7. Situações do empréstimo e do objeto

### 7.1 Situações do empréstimo

| Situação Atual | Evento | Próxima Situação | Regra / Condição |
| :--- | :--- | :--- | :--- |
| Nenhuma | Criar Empréstimo | Ativo | Objeto deve estar com situação "Disponível" e estado de conservação diferente de "Inutilizado" (RN01); amigo não pode estar arquivado (RN03). |
| Ativo | Data atual ultrapassa 23:59:59 do prazo | Atrasado | Processado automaticamente na virada do dia (RN08). |
| Ativo | Registrar Devolução Parcial | Ativo | Apenas para conjuntos; saldo pendente registrado no histórico (RN15). |
| Ativo | Registrar Devolução (total) | Encerrado | Registro da data de devolução e do estado de devolução (RN13). |
| Ativo | Solicitar Renovação | Ativo | Limite de renovações < 3 (RN11); nova data = MAX(prazo atual, data atual + dias) e dentro de 365 dias da saída (RN07, RN12). |
| Ativo | Marcar como Perdido | Perdido | Ação manual do Dono, a qualquer momento (RN16). |
| Atrasado | Registrar Devolução Parcial | Atrasado | Apenas para conjuntos; saldo pendente registrado no histórico (RN15). |
| Atrasado | Registrar Devolução (total) | Encerrado | Registro da data de devolução e do estado de devolução (RN13). |
| Atrasado | Solicitar Renovação | Ativo | Limite de renovações < 3 (RN11); nova data = MAX(prazo atual, data atual + dias) e dentro de 365 dias da saída (RN07, RN12). A situação retorna para Ativo. |
| Atrasado | Marcar como Perdido | Perdido | Ação manual do Dono, a qualquer momento; a sugestão é sinalizada a partir de 30 dias de atraso (RN16). |
| Encerrado | (nenhum) | Encerrado | Situação final; o registro não pode ser alterado. |
| Perdido | (nenhum) | Perdido | Situação final; o registro não pode ser alterado. A recuperação do objeto é tratada na situação do objeto (7.2, RN17). |

### 7.2 Situações do objeto

| Situação Atual | Evento | Próxima Situação | Regra / Condição |
| :--- | :--- | :--- | :--- |
| Disponível | Criar Empréstimo | Emprestado | Objeto com estado de conservação diferente de "Inutilizado" (RN01). |
| Emprestado | Empréstimo Encerrado | Disponível | Aplica os efeitos do estado de devolução (RN14). Com "Inutilizado", o objeto fica fora de novos empréstimos até edição manual do estado. |
| Emprestado | Empréstimo declarado Perdido | Perdido | Ação manual do Dono (RN16); inclui perda do saldo pendente de conjuntos (RN15). |
| Perdido | Registrar Recuperação | Disponível | Ajuste do tipo "Recuperação" no histórico; empréstimo original permanece "Perdido" (RN17). |
| Disponível | Arquivar | Arquivado | Objeto sem empréstimo ativo ou atrasado (RN03). |
| Arquivado | Desarquivar | Disponível | Ação manual do Dono (RN03). |

---

## 8. Modelo conceitual de informação

Este modelo descreve as entidades de negócio, suas informações e seus relacionamentos, sem definir forma de armazenamento.

### 8.1 Entidade: Amigo
Guarda as informações das pessoas para quem o Dono empresta pertences.

| Informação | Tipo conceitual | Obrigatória? | Descrição / Relacionamento |
| :--- | :--- | :--- | :--- |
| Identificador | Número inteiro | Sim | Identifica unicamente cada amigo. |
| Nome | Texto | Sim | Nome completo do amigo. Substituído por "Contato Removido" na anonimização (RN04). |
| Telefone | Texto | Sim | Telefone de contato. Apagado na anonimização (RN04). |
| E-mail | Texto | Não | Endereço de e-mail opcional. |
| Arquivado | Sim/Não | Sim | Indica se o amigo está arquivado (RN03). Padrão: Não. |

### 8.2 Entidade: Objeto
Guarda os itens cadastrados no inventário pessoal.

| Informação | Tipo conceitual | Obrigatória? | Descrição / Relacionamento |
| :--- | :--- | :--- | :--- |
| Identificador | Número inteiro | Sim | Identifica unicamente cada objeto. |
| Nome | Texto | Sim | Nome descritivo do item. |
| Categoria | Texto | Sim | Categoria (Ferramentas, Livros, Eletrônicos, Jogos, Outros). |
| Foto | Imagem de referência | Não | Imagem opcional do objeto. |
| Estado de conservação | Texto | Sim | Estado atual (Novo, Bom, Desgastado, Danificado, Inutilizado). |
| Situação | Texto | Sim | Situação do objeto no inventário (Disponível, Emprestado, Perdido, Arquivado). Padrão: Disponível. |

### 8.3 Entidade: Empréstimo
Registra a movimentação de saída e retorno de um objeto.

| Informação | Tipo conceitual | Obrigatória? | Descrição / Relacionamento |
| :--- | :--- | :--- | :--- |
| Identificador | Número inteiro | Sim | Identifica unicamente cada empréstimo. |
| Objeto | Referência | Sim | Relaciona-se a um Objeto. |
| Amigo | Referência | Sim | Relaciona-se a um Amigo (Tomador). |
| Data de saída | Data | Sim | Data de início do empréstimo. |
| Prazo de devolução | Data | Sim | Data limite vigente para devolução (atualizada a cada renovação; máximo de data de saída + 365 dias, RN07). |
| Data de devolução | Data | Não | Data real da devolução total (registrada ao encerrar). |
| Estado de devolução | Texto | Não | Estado do item na devolução (Sem Alteração, Dano Leve, Dano Grave, Inutilizado). |
| Situação | Texto | Sim | Situação do empréstimo (Ativo, Atrasado, Encerrado, Perdido). |

### 8.4 Entidade: Renovação
Histórico de prorrogações dos empréstimos.

| Informação | Tipo conceitual | Obrigatória? | Descrição / Relacionamento |
| :--- | :--- | :--- | :--- |
| Identificador | Número inteiro | Sim | Identifica unicamente cada renovação. |
| Empréstimo | Referência | Sim | Relaciona-se a um Empréstimo. |
| Dias adicionados | Número inteiro | Sim | Quantidade de dias informada pelo Dono na renovação (mínimo 1). |
| Nova data de prazo | Data | Sim | Nova data limite definida: MAX(prazo atual, data atual + dias adicionados) (RN12). |
| Data e hora da renovação | Data e hora | Sim | Momento exato do registro da renovação. |

### 8.5 Entidade: Ajuste de Histórico
Registra eventos de ajuste que não alteram o empréstimo original: devoluções parciais (RN15) e recuperação de objetos perdidos (RN17).

| Informação | Tipo conceitual | Obrigatória? | Descrição / Relacionamento |
| :--- | :--- | :--- | :--- |
| Identificador | Número inteiro | Sim | Identifica unicamente cada ajuste. |
| Objeto | Referência | Sim | Relaciona-se a um Objeto. |
| Empréstimo | Referência | Sim | Relaciona-se a um Empréstimo. |
| Tipo | Texto | Sim | Tipo do ajuste (Devolução Parcial, Recuperação). |
| Descrição | Texto | Não | Descrição do saldo pendente (Devolução Parcial) ou observação sobre o estado do objeto recuperado (Recuperação). |
| Data e hora do registro | Data e hora | Sim | Momento exato do registro do ajuste. |

---

## 9. Casos de borda e exceções

| Situação | Comportamento Esperado | Regra |
| :--- | :--- | :--- |
| Tentar excluir um amigo ou objeto com histórico de empréstimo. | A exclusão é bloqueada e a solução informa: "Contato possui histórico e não pode ser excluído. Deseja arquivá-lo?" (para objetos, mensagem equivalente). O item arquivado permanece no histórico e deixa de ser oferecido em novos empréstimos. | RN03 |
| Tentar arquivar ou excluir amigo/objeto com empréstimo ativo ou atrasado. | A ação é bloqueada e a solução informa que há empréstimos em andamento. | RN03 |
| Mudança de fuso horário do Dono (ex.: viagem). | Todas as contagens de prazo, vencimento e atraso adotam o fuso de referência oficial `America/Sao_Paulo`, independentemente de onde o Dono esteja. | RN06, RN08 |
| Objeto devolvido com `Dano Leve`. | O empréstimo é encerrado, o objeto volta para `Disponível` com o estado de conservação inalterado, e o dano é registrado no histórico com alerta informativo (não bloqueante) em novas escolhas. | RN13, RN14 |
| Objeto devolvido com `Dano Grave`. | O empréstimo é encerrado, o estado de conservação passa automaticamente para `Danificado` e o objeto volta para `Disponível`, com o dano registrado no histórico. | RN13, RN14 |
| Objeto devolvido com estragos totais (`Inutilizado`). | O empréstimo é encerrado, mas o objeto fica indisponível para novos empréstimos até que o Dono edite manualmente o estado do item. | RN01, RN13, RN14 |
| Amigo solicita exclusão de seus dados (LGPD). | A solução executa a anonimização dos dados cadastrais do amigo, preservando a integridade das estatísticas de empréstimos anteriores. | RN04 |
| Mudança de categoria do objeto com empréstimo ativo. | A alteração da categoria é permitida, porém o novo prazo padrão não afeta os empréstimos que já estão em andamento. | RN05 |
| Renovação feita antes do vencimento resultando em data menor ou igual ao prazo atual. | O prazo nunca é encurtado: a solução mantém a data vigente, informa que a renovação não amplia o prazo e não consome o contador de renovações. | RN12 |
| Renovação que ultrapassaria 365 dias acumulados desde a data de saída. | A renovação é bloqueada e a solução informa a data limite máxima permitida. | RN07, RN12 |
| Empréstimo em atraso há mais de 30 dias. | A solução sugere declarar o item como `Perdido`, sem alterar a situação automaticamente. O Dono também pode declarar a perda manualmente a qualquer momento, antes ou depois dos 30 dias. | RN16 |
| Devolução parcial de conjunto. | O empréstimo permanece `Ativo` ou `Atrasado` e o objeto permanece `Emprestado`; o saldo pendente é registrado em texto no histórico, sem dividir o objeto. | RN02, RN15 |
| Saldo pendente de conjunto declarado como perdido. | O empréstimo e o objeto passam para `Perdido`, preservando no histórico as partes já devolvidas. | RN15, RN16 |
| Objeto `Perdido` é recuperado. | O Dono registra um ajuste do tipo `Recuperação`; o objeto volta para `Disponível` e o empréstimo original permanece `Perdido`. | RN17 |
| Dono sem canal de lembretes disponível (recebimento desativado ou não habilitado). | A solução orienta o Dono a reativar os lembretes e mantém os indicadores visuais de prazo nas consultas de empréstimos. | RN09 |

---

## 10. Atributos de qualidade e restrições do projeto
* **Tempo de resposta**: A listagem de empréstimos e as buscas no histórico devem responder em no máximo 1,5 segundo para bases com até 5.000 registros.
* **Privacidade e Proteção de Dados (LGPD)**: Os dados pessoais dos contatos devem ser mantidos sob proteção adequada, com acesso restrito ao Dono. A anonimização deve estar disponível sob demanda.
* **Contexto de uso**: Uso individual e cotidiano, com registros rápidos no momento do empréstimo e consultas frequentes em situações do dia a dia.
* **Acessibilidade**: A experiência deve atender ao padrão WCAG 2.1 nível AA: contraste mínimo de 4,5:1 para textos em relação ao fundo Vanilla Cream (`#FFF7E6`) e de 3:1 para bordas e ícones. Nenhuma informação (como situação de atraso) pode ser transmitida apenas por cor (ver seção 10.2).

### 10.1 Inspiração de Design e Identidade Visual (Library Card Aesthetic)
O conceito visual e a experiência de uso do projeto "Empresta Aí" são profundamente inspirados na estética nostálgica, aconchegante e analógica de um **cartão de biblioteca tradicional vintage**.

A identidade deve transmitir a sensação de toque de papel antigo, carimbos e anotações manuais, traduzidos para uma experiência elegante e limpa (*cozy productivity*):
* **Tipografia e Layout:** Uso de fontes serifadas clássicas e elegantes nos títulos (lembrando carimbos ou impressões de máquinas de escrever antigas) combinadas com fontes monoespaçadas ou sem serifa limpas para os dados tabulares. A apresentação das informações imita o formato de fichas pautadas e tabelas de controle clássicas de acervos literários.
* **Paleta de Cores Coesa:** Baseada em tons terrosos suaves, cremes e rosados acinzentados (como o fundo de papel envelhecido *Vanilla Cream*, detalhes em rosa queimado/marrom *Rosewood* e acentos suaves *Blush Petal*).
* **Elementos Visuais Analógicos:** Presença visual simulada de grampos metálicos de fixação superior (`paperclips`), carimbos circulares de data ou de situação, e anotações manuscritas estilizadas nos registros de empréstimo, transformando a rotina de controle em uma experiência visual charmosa e prazerosa.
* **Legibilidade acima do estilo:** Elementos decorativos (grampos, carimbos, anotações manuscritas) não podem ser a única forma de transmitir uma informação e não devem reduzir o contraste dos textos; fontes manuscritas são restritas a elementos ornamentais.

### 10.2 Paleta de Cores (WCAG AA)
A identidade utiliza a paleta abaixo, com os tons ajustados para atender ao contraste mínimo de 4,5:1 (texto) em relação ao fundo Vanilla Cream (`#FFF7E6`). Razões calculadas pela fórmula de luminância relativa do WCAG 2.1.

| Papel | Cor | HEX | Contraste com `#FFF7E6` | Uso |
| :--- | :--- | :--- | :--- | :--- |
| Fundo primário | **Vanilla Cream** | `#FFF7E6` | — | Fundo principal, superfícies claras e fichas de leitura. |
| Texto principal | **Tinta Café** | `#4A2C2A` | 11,69:1 | Textos, dados tabulares e ícones sobre o fundo. |
| Acento primário | **Rosewood Profundo** (ajuste do Rosewood original) | `#8A444D` | 6,53:1 | Elementos de ação principal (texto `#FFF7E6` sobre o elemento: 6,53:1), cabeçalhos, textos/ícones de alerta e situação `Atrasado`. |
| Secundária / destaque | **Blush Petal** | `#F7C8D3` | 1,39:1 (somente superfície) | Fundo de etiquetas da situação `Ativo` e de elementos de ação secundária. Nunca usada como cor de texto. Textos sobre ela usam Tinta Café (8,40:1) e o componente recebe borda Rosewood Profundo (6,53:1). |
| Decorativa | **Rosewood Original** | `#B46A72` | 3,76:1 | Somente elementos decorativos e gráficos não textuais (ilustrações, linhas pautadas, carimbos). Reprovada para texto (mínimo 4,5:1); nunca usada em textos. |
| Perdido | **Rosewood Escuro** | `#5C2E35` | 10,38:1 | Fundo da etiqueta `Perdido` com texto `#FFF7E6` (10,38:1). |

**As situações nunca dependem apenas da cor.** Cada situação é sempre apresentada com ícone e texto visíveis, além da cor:

| Situação | Cor de apoio | Ícone obrigatório | Texto visível |
| :--- | :--- | :--- | :--- |
| Ativo | Fundo Blush Petal, texto Tinta Café, borda Rosewood Profundo | Relógio | "Ativo" e prazo (ex.: "Vence em 10/11") |
| Atrasado | Fundo Rosewood Profundo, texto Vanilla Cream | Triângulo de alerta | "Atrasado há N dia(s)" (e carimbo "ATRASADO" nos detalhes) |
| Encerrado | Fundo Vanilla Cream, texto e borda Tinta Café | Marca de verificação | "Encerrado" |
| Perdido | Fundo Rosewood Escuro, texto Vanilla Cream | Círculo com X | "Perdido" |
| Dano Leve / Dano Grave / Inutilizado | Texto Tinta Café, borda Rosewood Profundo | Ícone de aviso | Texto exato do estado (`Dano Leve`, `Dano Grave`, `Inutilizado`) |

---

## 11. Decisões e premissas

| ID | Decisão | Alternativas Consideradas | Justificativa |
| :--- | :--- | :--- | :--- |
| DEC01 | O Tomador não possui acesso ou conta na solução. | Criar solução multiusuário para Dono e Tomador, com canal de mensagens entre eles. | Reduz radicalmente a complexidade do projeto, elimina a necessidade de comunicação entre usuários e foca no uso estritamente pessoal do Dono. |
| DEC02 | Contagem de prazos em dias fixos com vencimento até 23:59:59. | Permitir escolha de hora exata de devolução (ex: 14:30). | Simplifica a experiência do usuário e evita falsos alarmes de atraso durante o horário comercial/social. |
| DEC03 | Limite rígido de 3 renovações por empréstimo. | Renovações ilimitadas ou sem renovação. | Evita que o objeto fique emprestado indefinidamente sem que haja uma tomada de decisão (devolução ou perda). |
| DEC04 | Anonimização dos dados do amigo em caso de solicitação LGPD. | Exclusão em cascata de todo o histórico do amigo. | Mantém o histórico do inventário do Dono consistente sem violar o direito de remoção de dados pessoais do contato. |
| DEC05 | A sugestão de 30 dias de atraso é apenas um alerta; o Dono pode declarar `Perdido` a qualquer momento. | Marcar como perdido automaticamente após 30 dias; só permitir a ação após 30 dias. | Preserva a autonomia do Dono (que sabe quando um item está realmente perdido) e evita mudanças automáticas de situação sem confirmação. |
| DEC06 | Renovação usa `MAX(prazo atual, data atual + dias)` e não consome renovação quando não amplia o prazo. | Sempre somar os dias à data da renovação. | Impede que uma renovação antecipada encurte o prazo já concedido e evita desperdiçar o limite de 3 renovações. |
| DEC07 | O limite de 365 dias (RN07) vale para o prazo acumulado desde a data de saída, incluindo renovações. | Aplicar 365 dias a cada renovação; limite apenas no prazo inicial. | Mantém um teto objetivo e coerente com a DEC03, evitando empréstimos de duração indefinida por renovações sucessivas. |
| DEC08 | Devolução parcial é registrada como ajuste descritivo (entidade Ajuste de Histórico), sem dividir o conjunto. | Cadastrar cada item do conjunto como objeto separado; controle numérico de saldo. | Respeita o conceito de conjunto indivisível (RN02) e mantém o modelo de informação simples. |
| DEC09 | A recuperação de objeto perdido gera um ajuste no histórico e não reabre o empréstimo. | Reabrir o empréstimo `Perdido`; editar a situação do empréstimo. | Mantém imutáveis os empréstimos finalizados e preserva a rastreabilidade da perda e da recuperação. |
| DEC10 | Tons da paleta ajustados para WCAG AA, com ícones e textos em todas as situações. | Manter os tons originais (`#B46A72`, `#F7C8D3`) para texto e indicar situação somente por cor. | Garante legibilidade (Rosewood original tem 3,76:1) e acessibilidade para pessoas com daltonismo ou baixa visão. |
