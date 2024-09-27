# Tutorial: Configurando o Ambiente C no Visual Studio Code (VSCode)

### Instalando Extensões para Programação em C

Abra o Visual Studio Code e adicione as seguintes extensões:

- **C/C++** (Microsoft): Para suporte básico à linguagem C/C++ e depuração.  
  Instale a extensão via [link para marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools).
  <img width="1092" alt="image" src="https://github.com/user-attachments/assets/ab03f511-66c3-4e56-8e49-83379647b271">


- **C/C++ Compile Run**: Para facilitar a compilação e execução do código C/C++ diretamente no VSCode.  
  Instale a extensão via [link para marketplace](https://marketplace.visualstudio.com/items?itemName=danielpinto8zz6.c-cpp-compile-run).
  <img width="1346" alt="image" src="https://github.com/user-attachments/assets/559ced6d-7be3-4c89-8164-cd88309916d5">


### Baixando e Instalando o MinGW

Você precisará de um compilador para C. O MinGW é uma excelente opção.

1. Baixe o instalador do MinGW aqui:  
   [https://sourceforge.net/projects/mingw/](https://sourceforge.net/projects/mingw/)
2. Durante a instalação, selecione os componentes `mingw32-gcc-g++` e `mingw32-gcc-objc`.
3. ![image](https://github.com/user-attachments/assets/9dc4e976-ae8d-495d-a1ab-44363108f2e0)


### Adicionando MinGW às Variáveis de Ambiente

Para poder compilar o código diretamente no VSCode, é necessário configurar o caminho do MinGW nas variáveis de ambiente do Windows.

1. Na barra de pesquisa do Windows, procure por **"variáveis"**.
2. Escolha a opção **"Editar as variáveis de ambiente para sua conta"**.
3. Na janela que abrir, selecione a variável **"Path"** e clique em **Editar**.
4. Adicione o seguinte caminho à lista:
   ```
   C:\MinGW\bin
   ```
5. Confirme as alterações e feche a janela.

Agora, o ambiente está configurado!

### Compilando e Executando o Código no VSCode

Com o código-fonte em C aberto no VSCode, ao pressionar `F6`, o código será compilado e executado automaticamente no terminal integrado do VSCode.

---
![image](https://github.com/user-attachments/assets/98c5c3d3-5d6b-4f09-8c90-04e9ed060ef1)

# Código C: Resolver Sudoku (teste o ambiente)

Aqui está um exemplo de código em C para resolver um Sudoku utilizando backtracking.

```c
#include <stdio.h>

#define N 9

// Função para imprimir a grade do Sudoku
void printSudoku(int grid[N][N]) {
    for (int row = 0; row < N; row++) {
        for (int col = 0; col < N; col++)
            printf("%2d", grid[row][col]);
        printf("\n");
    }
}

// Função para verificar se é seguro colocar um número na célula (row, col)
int isSafe(int grid[N][N], int row, int col, int num) {
    // Verificar a linha
    for (int x = 0; x < N; x++)
        if (grid[row][x] == num)
            return 0;

    // Verificar a coluna
    for (int x = 0; x < N; x++)
        if (grid[x][col] == num)
            return 0;

    // Verificar o bloco 3x3
    int startRow = row - row % 3, startCol = col - col % 3;
    for (int i = 0; i < 3; i++)
        for (int j = 0; j < 3; j++)
            if (grid[i + startRow][j + startCol] == num)
                return 0;

    return 1;
}

// Função para resolver o Sudoku usando backtracking
int solveSudoku(int grid[N][N], int row, int col) {
    // Se chegamos ao fim da última linha, o Sudoku está resolvido
    if (row == N - 1 && col == N)
        return 1;

    // Se a coluna excede o limite, movemos para a próxima linha
    if (col == N) {
        row++;
        col = 0;
    }

    // Ignorar as células que já estão preenchidas
    if (grid[row][col] != 0)
        return solveSudoku(grid, row, col + 1);

    // Tentar números de 1 a 9
    for (int num = 1; num <= 9; num++) {
        // Verificar se é seguro colocar o número na célula
        if (isSafe(grid, row, col, num)) {
            grid[row][col] = num;

            // Recursivamente tentar preencher o restante do grid
            if (solveSudoku(grid, row, col + 1))
                return 1;
        }

        // Se a tentativa falhar, desfazemos a atribuição
        grid[row][col] = 0;
    }

    return 0;
}

// Função principal
int main() {
    // Exemplo de Sudoku incompleto (0s representam células vazias)
    int grid[N][N] = {
        {5, 3, 0, 0, 7, 0, 0, 0, 0},
        {6, 0, 0, 1, 9, 5, 0, 0, 0},
        {0, 9, 8, 0, 0, 0, 0, 6, 0},
        {8, 0, 0, 0, 6, 0, 0, 0, 3},
        {4, 0, 0, 8, 0, 3, 0, 0, 1},
        {7, 0, 0, 0, 2, 0, 0, 0, 6},
        {0, 6, 0, 0, 0, 0, 2, 8, 0},
        {0, 0, 0, 4, 1, 9, 0, 0, 5},
        {0, 0, 0, 0, 8, 0, 0, 7, 9}
    };

    if (solveSudoku(grid, 0, 0))
        printSudoku(grid);
    else
        printf("Nenhuma solução encontrada\n");

    return 0;
}
```
