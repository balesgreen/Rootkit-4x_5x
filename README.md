# Rootkit - Kernel 4.x | 5.x
  
   * Rootkit -> SysCall mkdir hook

  <img src="https://imgur.com/NH5w62K.png" />
  
  
  <h3>Documentando sobre a estrutura do Rootkit.</h3>
  
  <img src="https://imgur.com/5RtnDhT.png" />
  
   * Essas são as bibliotecas importadas especificamente para trabalhar com o nosso RootKit. Todas serão essenciais para o nosso RootKit funcionar de certa forma. As bibliotecas especiais são:
    * kernel.h: Essa biblioteca é a parte principal do nosso código, pois é com ela que teremos possibilidade de mexer com o kernel do nosso sistema.
    * syscall.h: Essa biblioteca é bem pequena e com ela nós conseguimos invocar chamadas no sistema.
    * module.h: Essa biblioteca é a parte do nosso código onde interfere na criação do módulo do kernel, pois um Rootkit é apenas um módulo que será carregado no Kernel.
