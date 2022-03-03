(prepC)=
# Preparação do ambiente de trabalho usando C

Exemplos de código obtidos em [LearnOpenGL](https://learnopengl.com/Getting-started/Creating-a-window)

## Pré-requisitos

Antes de iniciar, verifique que tem os *drivers* da carta gráfica atualizados.

Este guia pressupõe a instalação prévia do compilador de C/C++ da GNU, da biblioteca [glfw3](http://www.glfw.org/download.html), e do sistema de apoio à compilação [cmake](https://cmake.org/download/).

## Instruções
### Ano letivo 2020-21

Pode seguir as [instruções](./P1-old.tgz) preparadas pelo Professor Sérgio Jesus no ano letivo 2020-21.

### Passo a passo em Linux

Passo 0. Criar uma pasta para as práticas na nossa pasta de *CGr-2021-22*.

```bash
user@machine:~$ cd CGr-2021-22; mkdir P; cd P
```

Passo 1. Vamos precisar de uma diretoria para a biblioteca *glad* e outras para cada um dos trabalhos.

Passo 1.1. Criemos então duas pastas, *glad* e *exemplo1*. 

```bash
user@machine:~/CGr-2021-22/P$ mkdir glad; mkdir exemplo1
```

Passo 1.2. Na pasta global, vamos criar um *CMakeLists.txt* que fará a ligação a cada um dos trabalhos.

```bash
user@machine:~/CGr-2021-22/P$ touch CMakeLists.txt
```

```cmake
# CMakeLists files in this project can
# refer to the root source directory of the project as ${CGr_SOURCE_DIR} and
# to the root binary directory of the project as ${CGr_BINARY_DIR}.
cmake_minimum_required (VERSION 3.15)
project (CGr)

# OpenGL Flags
set(OpenGL_GL_PREFERENCE GLVND)

# GLFW - Installed via: sudo apt install libglfw3 libglfw3-dev
option(GLFW_BUILD_DOCS OFF)
option(GLFW_BUILD_EXAMPLES OFF)
option(GLFW_BUILD_TESTS OFF)
option(GLFW_INSTALL OFF)
find_package(glfw3 REQUIRED)

# Recurse into the "Hello" and "Demo" subdirectories. This does not actually
# cause another cmake executable to run. The same process will walk through
# the project's entire directory structure.
add_subdirectory (glad)
add_subdirectory (exemplo1)
```

Passo 2. Obter e preparar o *glad*.

Passo 2.1. Desloquemo-nos para a pasta *glad* 

```bash
user@machine:~/CGr-2021-22/P$ cd glad
```

e aí descarreguemos o zip para a nossa [configuração](http://glad.dav1d.de/#profile=core&specification=gl&api=gl%3D3.3&api=gles1%3Dnone&api=gles2%3Dnone&api=glsc2%3Dnone&language=c&loader=on) escolhendo *Generate* guardando o zip produzido.

Passo 2.2. Abrir o zip com

```bash
user@machine:~/CGr-2021-22/P/glad$ unzip glad-core.zip 
```

Passo 2.3. Podemos eliminar o zip, já não precisaremos dele.

```bash
user@machine:~/CGr-2021-22/P/glad$ rm glad-core.zip 
```

Passo 2.4 Criemos o CMakeLists.txt com as instruções para criar esta biblioteca.

```bash
user@machine:~/CGr-2021-22/P/glad$ touch CMakeLists.txt
```

```cmake
# Create a library called "glad" which includes the source file "glad.c".
# The extension is already found. Any number of sources could be listed here.
add_library (glad src/glad.c)

# Make sure the compiler can find include files for our glad library
# when other libraries or executables link to glad
target_include_directories (glad PUBLIC include)

```

O *glad* está preparado e, de futuro, não precisaremos repetir estes passos.

Passo 3. Vamos criar agora o primeiro exemplo.

Passo 3.1 Desloquemo-nos para a pasta *exemplo1* e coloquemos aí o código fonte, *main.cpp*

```bash
user@machine:~/CGr-2021-22/P/glad$ cd ../exemplo1
```

```bash
user@machine:~/CGr-2021-22/P/exemplo1$ touch main.cpp
```

```c++
#include <glad/glad.h>
#include <GLFW/glfw3.h>

#include <iostream>

void framebuffer_size_callback(GLFWwindow* window, int width, int height);
void processInput(GLFWwindow *window);

// settings
const unsigned int SCR_WIDTH = 800;
const unsigned int SCR_HEIGHT = 600;

int main()
{
    // glfw: initialize and configure
    // ------------------------------
    glfwInit();
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

#ifdef __APPLE__
    glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE);
#endif

    // glfw window creation
    // --------------------
    GLFWwindow* window = glfwCreateWindow(SCR_WIDTH, SCR_HEIGHT, "LearnOpenGL", NULL, NULL);
    if (window == NULL)
    {
        std::cout << "Failed to create GLFW window" << std::endl;
        glfwTerminate();
        return -1;
    }
    glfwMakeContextCurrent(window);
    glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);

    // glad: load all OpenGL function pointers
    // ---------------------------------------
    if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
    {
        std::cout << "Failed to initialize GLAD" << std::endl;
        return -1;
    }    

    // render loop
    // -----------
    while (!glfwWindowShouldClose(window))
    {
        // input
        // -----
        processInput(window);

        // glfw: swap buffers and poll IO events (keys pressed/released, mouse moved etc.)
        // -------------------------------------------------------------------------------
        glfwSwapBuffers(window);
        glfwPollEvents();
    }

    // glfw: terminate, clearing all previously allocated GLFW resources.
    // ------------------------------------------------------------------
    glfwTerminate();
    return 0;
}

// process all input: query GLFW whether relevant keys are pressed/released this frame and react accordingly
// ---------------------------------------------------------------------------------------------------------
void processInput(GLFWwindow *window)
{
    if(glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)
        glfwSetWindowShouldClose(window, true);
}

// glfw: whenever the window size changed (by OS or user resize) this callback function executes
// ---------------------------------------------------------------------------------------------
void framebuffer_size_callback(GLFWwindow* window, int width, int height)
{
    // make sure the viewport matches the new window dimensions; note that width and 
    // height will be significantly larger than specified on retina displays.
    glViewport(0, 0, width, height);
}

```

Passo 3.2 Criemos o *CMakeLists.txt* com as instruções para criar o executável

```bash
user@machine:~/CGr-2021-22/P/exemplo1$ touch CMakeLists.txt
```

```cmake
# Add executable called "exemplo1" that is built from the source files
# "main.cxx". The extensions are automatically found.
add_executable (exemplo1 main.cpp)

# Link the executable to the glad library. Since the glad library has
# public include directories we will use those link directories when building
# exemplo1
target_link_libraries(exemplo1
  LINK_PUBLIC
    glfw
    glad
    ${CMAKE_DL_LIBS} 
)
```

Passo 4. Vamos então compilar o código deste exemplo. Voltemos para raiz das pastas das práticas

```bash
user@machine:~/CGr-2021-22/P/exemplo1$ touch cd ..
```

Passo 4.1. E criemos uma pasta *build* para os ficheiros resultantes e desloquemo-nos para aí.

```bash
user@machine:~/CGr-2021-22/P$ mkdir build; cd build
```

Passo 4.2. Usando o *cmake* produzimos *Makefile*, indicando onde está a raiz do *CMakeLists.txt* e onde queremos os executáveis. Neste exemplo

```bash
user@machine:~/CGr-2021-22/P/build$ cmake -S .. -B.
```

Passo 4.3. E determinamos a compilação

```bash
user@machine:~/CGr-2021-22/P/build$ make
```

Passo 5. Executamos o programa

```bash
user@machine:~/CGr-2021-22/P/build$ ./exemplo1/exemplo1 
```

e consegue-se uma janela preta

```{figure} /images/black-window.png
---
height: 300px
name: black-window
---
Primeiro exemplo
```

Passo 6. Novo exemplo, que ilustra o que faremos para cada novo programa.

Passo 6.1 No *CMakeLists.txt* principal, adicionamos outra pasta com

```cmake
add_subdirectory (exemplo2)
```

Passo 6.1 Duplicamos a pasta *exemplo1* para outra, ao mesmo nível, designada *exemplo2*

```bash
user@machine:~/CGr-2021-22/P$ cp -r exemplo1 exemplo2
```

Passo 6.2 Substituimos o *main.cpp* pelo novo código, neste caso *main1.2.cpp*

```c++
#include <glad/glad.h>
#include <GLFW/glfw3.h>

#include <iostream>

void framebuffer_size_callback(GLFWwindow* window, int width, int height);
void processInput(GLFWwindow *window);

// settings
const unsigned int SCR_WIDTH = 800;
const unsigned int SCR_HEIGHT = 600;

int main()
{
    // glfw: initialize and configure
    // ------------------------------
    glfwInit();
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);

#ifdef __APPLE__
    glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE);
#endif

    // glfw window creation
    // --------------------
    GLFWwindow* window = glfwCreateWindow(SCR_WIDTH, SCR_HEIGHT, "LearnOpenGL", NULL, NULL);
    if (window == NULL)
    {
        std::cout << "Failed to create GLFW window" << std::endl;
        glfwTerminate();
        return -1;
    }
    glfwMakeContextCurrent(window);
    glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);

    // glad: load all OpenGL function pointers
    // ---------------------------------------
    if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
    {
        std::cout << "Failed to initialize GLAD" << std::endl;
        return -1;
    }    

    // render loop
    // -----------
    while (!glfwWindowShouldClose(window))
    {
        // input
        // -----
        processInput(window);

        // render
        // ------
        glClearColor(0.5f, 0.5f, 0.5f, 1.0f);
        glClear(GL_COLOR_BUFFER_BIT);

        // glfw: swap buffers and poll IO events (keys pressed/released, mouse moved etc.)
        // -------------------------------------------------------------------------------
        glfwSwapBuffers(window);
        glfwPollEvents();
    }

    // glfw: terminate, clearing all previously allocated GLFW resources.
    // ------------------------------------------------------------------
    glfwTerminate();
    return 0;
}

// process all input: query GLFW whether relevant keys are pressed/released this frame and react accordingly
// ---------------------------------------------------------------------------------------------------------
void processInput(GLFWwindow *window)
{
    if(glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)
        glfwSetWindowShouldClose(window, true);
}

// glfw: whenever the window size changed (by OS or user resize) this callback function executes
// ---------------------------------------------------------------------------------------------
void framebuffer_size_callback(GLFWwindow* window, int width, int height)
{
    // make sure the viewport matches the new window dimensions; note that width and 
    // height will be significantly larger than specified on retina displays.
    glViewport(0, 0, width, height);
}
```

Passo 6.3. Corrigimos o nome do executável e do código fonte no ficheiro *CMakeLists.txt* da pasta *exemplo2*

Passo 7. Para compilar e executar

Passo 7.1 Voltamos para a pasta dos resultados, e chamamos o *cmake* e o *make*.

```bash
user@machine:~/CGr-2021-22/P/build$ cmake -S .. -B. ; make
```

Se algo não correu bem, após corrigirmos o código fonte, bastará o *make*.

```bash
user@machine:~/CGr-2021-22/P/build$ make
```

Passo 7.2. Executamos o programa

```bash
user@machine:~/CGr-2021-22/P/build$ ./exemplo2/exemplo2 
```

o que faz surgir uma janela cinzenta.

```{figure} /images/grey-window.png
---
height: 300px
name: grey-window
---
Segundo exemplo