# semana-13
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <title>CRUD Agendamento de Consultas</title>

</head>

<body>
    
    <div class="container">

        <h1>Sistema de Agendamento</h1>
        <input type="text" id="nome" placeholder="Nome Completo">
        <input type="text" id="telefone" placeholder="Telefone">
        <input type="text" id="especialidade" placeholder="Especialidade">
        <button class="salvae" onclick="salvarAgendamento()">
            Salvar Agendamento
        </button>
        
        <button class="baixar" onclick="baixartxt()">
            Baixar Arquivo TXT
        </button>
        
        <table>

            <thead>
                <tr>
                    <th>Nome</th>
                    <th>Telefone</th>
                    <th>Especialidade</th>
                    <th>Ações</th>
                </tr>
            </thead>

            <tbody id="listaAgendamentos">
                
            </tbody>

        </table>

    </div>

    <script>

        let indexEdicao = -1;

        // CARREGAR DADOS

        function carregaAgendamentos() {

            const agendamento=
                JSON.parse(localStorage.getItem("agendamentos")) || [];
            
            const tabela =
             document.getElementById("listaAgendamentos");
            
            tabela.innerHTML = "";

            agendamentos.forEach((agendamento, index) => {

                tabela.innerHTML += `
                    <tr>

                        <td>${agendamento.nome}</td>

                        <td>${agendamento.telefone}</td>

                        <td>${agendamento.especialidade}</td>
                        
                        <td>

                            <button class="editar"
                             onclick="editarAgendamento(${index})">
                                Editar
                            </button>

                            <button class="excluir"
                             onclick="excluirAgendamento(${index})">
                                Excluir
                            </button>

                        </td>

                    </tr>
                `;

            });

        }

        // SALVAR AGENDAMENTO

        function salvarAgendamento() {

            const nome = document.getElementById("nome").value;

            const telefone = document.getElementById("telefone").value;

           const especialidade = document.getElementById("especialidade").value;

            if (nome == "" || telefone == "" || especialidade == "") {
                alert("Preencha todos os campos");
                return;

            }

            let agendamentos = JSON.parse(localStorage.getItem("agendamentos")) || [];

            const novoAgendamento = {
                nome,
                telefone,
                especialidade
            };

            // ALTERAR
            if (indexEdicao >= 0) {

                agendamentos[indexEdicao] = novoAgendamento;
                
                indexEdicao = -1;

            } else {

                // NOVO CADASTRO
                agendamentos.push(novoAgendamento);

            }

            localStorage.setItem(
                "agendamentos",
                JSON.stringify(agendamentos)
            );

            limparCampos();

            carregaAgendamentos();

        }

        // EDITAR
        function editarAgendamento(index) {

            let agendamentos = 
                JSON.parse(localStorage.getItem("agendamentos")) || [];

            document.getElementById("nome").value = agendamentos[index].nome;
            document.getElementById("telefone").value = agendamentos[index].telefone;
            document.getElementById("especialidade").value = agendamentos[index].especialidade;

            indexEdicao = index;

        }

        // EXCLUIR
        function excluirAgendamento(index) {

            let agendamentos =  
                JSON.parse(localStorage.getItem("agendamentos")) || [];

            agendamentos.splice(index, 1);

            localStorage.setItem(
                "agendamentos",
                JSON.stringify(agendamentos)
            );

            carregaAgendamentos();

        }

        // LIMPAR CAMPOS

        function limparCampos() {

            document.getElementById("nome").value = "";

            document.getElementById("telefone").value = "";

            document.getElementById("especialidade").value = "";
        
        }

        // GERAR TXT
        function baixarTXT() {
            const agendamentos = 
                  JSON.parse(localStorage.getItem("agendamentos")) || [];

            if (agendamentos.length == 0) {
                alert("Nenhum agendamento cadastrado");
                return;
            }

            let texto = "--- RELATÓRIO AGENDAMENTOS ---\n\n";

            agendamentos.forEach((agendamento, index) => {
               
                texto += `
                    Agendamento ${index + 1}:
                    Nome: ${agendamento.nome}
                    Telefone: ${agendamento.telefone}
                    Especialidade: ${agendamento.especialidade}\n\n `;
            
                });

            const blob = new Blob(
                [texto],
                { type: "text/plain" }
            );

            const link = 
            document.createElement("a")

            link.href =
            URL.createObjectURL(blob);

            link.download = 
            "agendamento,txt";

            link.click();

        }

        // INICIAR SISTEMA
        
        carregaAgendamentos();

    </script>

</body>
</html>
