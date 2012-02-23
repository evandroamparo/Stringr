﻿- Não sensível a maiúsculas/minúsculas
- Caracteres de início e fim das tags: "{" e "}"

Parâmetros comuns:
{nome_do_parâmetro [atributo=valor[ atributo=valor]]}
O nome do parâmetro não pode conter espaços mas pode conter caracteres acentuados(?), desde
que seja referenciado da mesma forma como está definido no template.
Recomenda-se usar nomes semelhantes a identificadores no código fonte.
Exemplos: Nome, CodigoCliente.

Parâmetros especiais:

{Date|Time|DateTime [atributo=valor[ atributo=valor]]}
Date gera a data atual;
Time gera a hora atual;
DateTime gera a data e hora atuais.

Atributos gerais:
- Length: tamanho máximo da string. Número inteiro positivo.
Se a string resultante tive comprimento maior que o valor de Tamanho, ela será truncada.
- Case: Upper|Lower: determina se o valor do parâmetro será convertido para maiúsculas
ou minúsculas. A ausência deste atributo mantem o valor original.

Atributos de data/hora:
- Format: string de formatação de data e hora, segundo a sintaxe do Pascal.

Em todos os casos, se o valor do atributo contiver espaços, deve ser delimitado por
aspas simples ('). Se for preciso incluir o caracter ' dentro do valor do atributo, use a
sequênicia \'.
Exemplos:
format=dd mm yyyy não é valido.
format='dd mm yyyy' é válido.
format='dd \'de\' mmmm \'de\' yyyy' é válido e equivale a "dd 'de' mmmm 'de' yyyy", que
resultaria, por exemplo, em "25 de janeiro de 2013".

- Listas (loops)
{list nome_da_lista}
 ...
{/list}

Uma lista determina uma região do template sujeita a repetições. Todo o conteúdo dentro
da lista será repetido até que o programa que utiliza o template sinalize o fim da lista.

Os nomes dos parâmetros da lista devem ter o seguinte formato:
nome_da_lista.nome_do_parâmetro
Isto significa que é possível ter um parâmetro {p} em qualquer lugar do template e um
parâmetro {nome_da_lista.p} somente dentro da lista de mesmo nome, sem que haja conflitos.
se um parâmetro {p} aparecer dentro de uma lista, será substituído pelo valor correspondente
como em qualquer outro lugar do texto. Os parâmetros refecenciados com o nome da lista
tem escopo local e seus valores devem ser atualizados a cada iteração ou permanecerão vazios.
Os atributos se aplicam a cada parâmetro individualmente e se mantem os mesmos a cada
iteração.

Exemplo:

{list Cliente}
Nome:       {Cliente.Nome}
Endereço:   {Cliente.Encereco length=10}

{/list}

Se esta lista tiver dois elementos, a saída será semelhante a esta:
Nome:       José da Silva
Endereço:   Rua Joaqui        --> truncado para 10 caracteres

Nome:       Pedro de Oliveira
Endereço:   Rio de Jan        --> truncado para 10 caracteres

------------------------------------------------

Exemplo de uso:

var
  Template: TStringr;
begin
  Template := TSTringr.Create('template.txt');
  Template['nome'].Valor := 'Evandro';
  Template.OnList = ProcessaLista;
end;

procedure TMinhaClasse.ProcessaLista(sender: TObject; Lista: TListaTemplate; var FimDaLista: boolean);
begin
  if (Lista.Nome = 'clientes') then
  begin
    Lista['nome'].Valor := 'José da Silva';
    FimDaLista := True; // o evento não será mais chamado
  end;
end;