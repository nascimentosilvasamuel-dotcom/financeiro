
${transacao.valor.toFixed(2)}`;
            colunaTipo.textContent = transacao.tipo === "entrada" ? "Entrada" : "Saída";

            const botaoExcluir = document.createElement("button");
            botaoExcluir.textContent = "Excluir";
            botaoExcluir.classList.add("button-delete");
            botaoExcluir.onclick = () => excluirTransacao(novaLinha, transacao.id);

            colunaAcoes.appendChild(botaoExcluir);

            saldo += transacao.tipo === "entrada" ? transacao.valor : -transacao.valor;
        });

        atualizarSaldo();
    }

    // Função para adicionar transação ao localStorage
    function adicionarTransacao() {
        const descricao = document.getElementById("descricao").value;
        const valor = parseFloat(document.getElementById("valor").value);
        const tipo = document.getElementById("tipo").value;

        if (!descricao || isNaN(valor) || valor <= 0) {
            alert("Por favor, insira uma descrição válida e um valor positivo.");
            return;
        }

        const transacao = {
            id: Date.now(),
            descricao: descricao,
            valor: valor,
            tipo: tipo
        };

        // Salvar transação no localStorage
        let transacoes = JSON.parse(localStorage.getItem('transacoes')) || [];
        transacoes.push(transacao);
        localStorage.setItem('transacoes', JSON.stringify(transacoes));

        // Atualiza a interface
        carregarTransacoes();

        // Limpa os campos
        document.getElementById("descricao").value = "";
        document.getElementById("valor").value = "";
    }

    // Função para excluir transação do localStorage
    function excluirTransacao(linha, id) {
        let transacoes = JSON.parse(localStorage.getItem('transacoes')) || [];
        transacoes = transacoes.filter(transacao => transacao.id !== id);

        localStorage.setItem('transacoes', JSON.stringify(transacoes));

        // Remove a linha da tabela
        tabelaTransacoes.deleteRow(linha.rowIndex - 1);

        // Atualiza o saldo
        saldo = 0;
        transacoes.forEach(transacao => {
            saldo += transacao.tipo === "entrada" ? transacao.valor : -transacao.valor;
        });

        atualizarSaldo();
    }

    // Função para atualizar o saldo na interface
    function atualizarSaldo() {
        saldoAtual.textContent = saldo.toFixed(2);
    }

    // Carregar transações ao iniciar a página
    window.onload = carregarTransacoes;

</script>

</body>
</html>
