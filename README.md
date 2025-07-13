#include <iostream>
#include <vector>
#include <string>
#include <limits>
#include<locale>

struct tarefa {
    std::string descricao;
    bool concluida;

    tarefa(const std::string& desc) : descricao(desc), concluida(false) {}
};

std::vector<tarefa> tarefas;

void adicionarTarefa() {
    std::cout << "\n---- Adicionar uma tarefa ----\n";
    std::cout << "Digite a descrição da tarefa: ";
    std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
    std::string descricao;
    std::getline(std::cin, descricao);

    tarefas.emplace_back(descricao);
    std::cout << "Tarefa adicionada com sucesso!\n";
}
void visualizarTarefas() {
    std::cout << "\n --- SUAS TAREFAS ---\n";
    if (tarefas.empty()) {
        std::cout << "Nenhuma tarefa para exibir.\n";
        return;
    }

    for (size_t i = 0; i < tarefas.size(); ++i) {
        std::cout << (i + 1) << "."
            << (tarefas[i].concluida ? "[concluída] " : "[pendente] ")
            << tarefas[i].descricao << "\n";
    }
}

void marcaTarefaConcluida() {
    std::cout << "\n---- Marcar tarefa como concluída ----\n";
    visualizarTarefas();

    if (tarefas.empty()) {
        return;
    }
    int numeroTarefa;
    std::cout << "Digite o número da tarefa concluída: ";
    std::cin >> numeroTarefa;

    if (std::cin.fail() || numeroTarefa < 1 || numeroTarefa > static_cast<int>(tarefas.size())) {
        std::cout << "Número inválido. Tente novamente.\n";
        std::cin.clear(); // Limpa o estado de erro
        std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n'); // Descarta a entrada inválida
        return;
    }
    tarefas[numeroTarefa - 1].concluida = true;
    std::cout << "\n--- Remove Tarefa--";
    visualizarTarefas(); // Mostra as tarefas após a atualização

}

void removerTarefa() {
    std::cout << "\n --- remover tarefa --- \n";
    visualizarTarefas(); // mostrar as tarefas para o usuario

    if (tarefas.empty()) {
        return; // Nenhuma tarefa para remover
    }

    int numeroTarefa;
    std::cout << "Digite o numero da tarefa a ser removida";
    std::cin >> numeroTarefa;

    if (std::cin.fail() || numeroTarefa < 1 || numeroTarefa > numeroTarefa > tarefas.size()) {
        std::cout << "Numero de Tarefa invalido! \n";
        std::cin.clear(); // limpa estado do error
        std::cin.clear();
        std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n'); //limpar o buffer
        return;
    }

    tarefas.erase(tarefas.begin() + (numeroTarefa - 1)); // remover a tarefa
    std::cout << "tarefa removida com sucesso! \n";

}

// FUNÇOES DE INTERFACE

// Funcao para exibir interface
void exbirMenu() {
    std::cout << "\n --- Gerenciador de Tarefas --- \n";
    std::cout << "1. Adicionar Tarefa\n";
    std::cout << "2. Visualizar Tarefas\n";
    std::cout << "3. Marca Tarefa como concluida \n";
    std::cout << "4. Remover Tarefa \n";
    std::cout << "5. Sair\n";
    std::cout << "Escolha uma opcao: ";
}

// Funcao principal do programa

int main() {
    setlocale(LC_ALL, "Portuguese");

    int escolha;

    do {
        exbirMenu();
        std::cin >> escolha;

        if (std::cin.fail()) {
            std::cout << "Entrada Invalida. Por favor, Digite um numero. \n";
            std::cin.clear();
            std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
            continue;
        }

        switch (escolha) {
        case 1:
            adicionarTarefa();
            break;
        case 2:
            visualizarTarefas();
            break;
        case 3:
            marcaTarefaConcluida();
            break;
        case 4:
            removerTarefa();
            break;
        case 5:
            std::cout << "Saindo do Gerenciador de Tarefas. Até mais!\n";
            break;
        default:
            std::cout << "Opcao invalida. tente novamente.\n";
            break;
        }
        std::cout << "\nPressione Enter para continuar...";
        std::cin.ignore(std::numeric_limits<std::streamsize > ::max(), '\n');
        std::cin.get();

    } while (escolha != 5);

    return 0;
}
