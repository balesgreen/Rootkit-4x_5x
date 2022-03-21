# Rootkit - Kernel 4.x | 5.x
  
   * Rootkit -> SysCall mkdir hook

  <img src="https://imgur.com/NH5w62K.png" />
  
  
  <h3>Documentando sobre a estrutura do Rootkit.</h3>
  
  <img src="https://imgur.com/5RtnDhT.png" />
  
   * Essas são as bibliotecas importadas especificamente para trabalhar com o nosso RootKit. Todas serão essenciais para o nosso RootKit funcionar de certa forma. As bibliotecas especiais são:
   
    * kernel.h: Essa biblioteca é a parte principal do nosso código, pois é com ela que teremos possibilidade de mexer com o kernel do nosso sistema.
    * syscall.h: Essa biblioteca é bem pequena e com ela nós conseguimos invocar chamadas no sistema.
    * module.h: Essa biblioteca é a parte do nosso código onde interfere na criação do módulo do kernel, pois um Rootkit é apenas um módulo que será carregado no Kernel.

  <img src="https://imgur.com/m8TVLvA.png" />
  
   * Aqui nós estamos então definindo algumas informações muito importantes para o nosso módulo e é sempre recomendável que a cada criação de um novo módulo, você deixe algumas descrições sobre o módulo. Após a montagem do nosso módulo, nós podemos conferir as seguintes informações utilizando <code>modinfo file.ko</code>
   
   <img src="https://imgur.com/aHYfTn5.png">
   
   # 
   
   <img src="https://imgur.com/Udh4jqH.png" />
   
  * Nessa parte do código, estou definindo a versão do Kernel e Arquitetura (x86_64) que será utilizada para que nossa proeza possa ser concluída com sucesso.
 
  <img src="https://imgur.com/aEjZdZu.png" />
  <br>
  <img src="https://imgur.com/s3JwRqH.png" />
  
  * Aqui nós entramos na parte principal do nosso código e é aqui que o nosso RootKit funcionará. Vamos detalhar a nossa estrutura para entendermos mais como funciona o nosso RootKit.

  <code>Bom, podemos notar que existe duas funções quase identicas que são separadas por dois pré-processadores "if/else". Após verificar a versão e arquitetura do kernel, PTREGS_SYSCALL_STUBS pode ou não ser definido. Se for, então definimos o ponteiro "orig_mkdir" da função e a declaração "hook_mkdir" da função para usar a estrutura "pt_regs". </code>
  
  <img src="https://imgur.com/yeKqK8Y.png" />
  
  ```c
  static struct ftrace_hook hook[] = {
    HOOK("sys_mkdir", hook_mkdir, &orig_mkdir),
};
  ```
  
  Como diz o nosso amigo Xcellerator: <code>A macro HOOK requer o nome da syscall ou função do kernel que estamos direcionando (sys_mkdir), a função de hook que escrevemos (hook_mkdir) e o endereço de onde queremos que a syscall original seja salva (orig_mkdir). Observe que hook[] pode conter mais do que apenas um único hook de função para rootkits mais complicados!

Uma vez que esta matriz é configurada, usamos fh_install_hooks() para instalar os hooks de função e fh_remove_hooks() para removê-los. Tudo o que temos a fazer é colocá-los nas funções init e exit respectivamente e fazer uma pequena verificação de erros:</code>
  
  * 
