# Alunos
Gabriel Pieruccini Knopp
Melchior Boaretto Neto

# Explicação de como o pai evita bloquear (papel do select() e do WNOHANG)
O pai evita bloquear pois a flag WNOHANG, usada na função "waitpid()" em "pool_reap()", instrui a função a retornar imediatamente, mesmo que nenhum processo filho tenha terminado, e a função "select()", por informar quantos processos filhos estão prontos, faz a função "pool_collect_ready()" retornar quando o resultado é nenhum, também evitando que o processo pai fique parado aguardando algum dos filhos estar pronto para continuar.

# Estratégia de fechamento de descritores e consequências de não fechar
Ao realizar o fork do processo pai, fecha-se o fd de leitura do processo filho e o fd de escrita do processo pai, criando um pipe unidirecional de escrita pelo filho para leitura pelo pai, até porque justamente os processos filhos estarão enviando informações ao pai, que será o responsável por as tratar e printar na tela. Não fechar acarreta, além do acúmulo de fds inutilizados abertos, no programa nunca acabar, pois a função "read()" do pai nunca alcança o EOF, já que só faz isso quando todos os escritores do pipe forem fechados - como um escritor ainda estaria aberto (o do pai), ele fica esperando por tempo indeterminado. 


# Declaração de uso de IA 
Foi utilizada IA apenas para certificar qual deveria ser o fd correto sendo lido na função "pool_collect_ready()", pois tivemos dificuldade para entender o papel do fd_set, a função "FD_ISSET()" e o que ela retornava. Fora isso, a confecção do código inteira foi feita sem a utilização de IA. Também tivemos que pesquisar sobre o que ocorre quando os descritores não são fechados e como as funções "select()" e a flag WNOHANG funcionam para completar relatório, mas sem o uso de IA.
