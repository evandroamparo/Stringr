- N�o sens�vel a mai�sculas/min�sculas
- Caracteres de in�cio e fim das tags: "{" e "}"

Par�metros comuns:
{nome_do_par�metro [atributo=valor[ atributo=valor]]}
O nome do par�metro n�o pode conter espa�os mas pode conter caracteres acentuados(?), desde
que seja referenciado da mesma forma como est� definido no template.
Recomenda-se usar nomes semelhantes a identificadores no c�digo fonte.
Exemplos: Nome, CodigoCliente.

Par�metros especiais:

{Date|Time|(Now?) [atributo=valor[ atributo=valor]]}
Geram a data ou hora atuais.

Atributos gerais:
- Size: tamanho m�ximo da string. N�mero inteiro positivo.
Se a string resultante tive comprimento maior que o valor de Tamanho, ela ser� truncada.
- Case: Upper|Lower: determina se o valor do par�metro ser� convertido para mai�sculas
ou min�sculas. A aus�ncia deste atributo mantem o valor original.

Atributos de data/hora:
- Format: string de formata��o de data e hora, segundo a sintaxe do Pascal.

Em todos os casos, se o valor do atributo contiver espa�os, deve ser delimitado por
aspas simples ('). Se for preciso incluir o caracter ' dentro do valor do atributo, use o
carcter de escape \ seguido de '.
Ex.: format='dd mm yyyy'
     format='dd\'mm\'yyyy'

- Listas (loops)
{list nome_da_lista}
 ...
{/list}

Uma lista determina uma regi�o do template sujeita a repeti��es. Todo o conte�do dentro
da lista ser� repetido at� que o programa que utiliza o template sinalize o fim da lista.

Os nomes dos par�metros da lista devem ter o seguinte formato:
nome_da_lista.nome_do_par�metro
Isto significa que � poss�vel ter um par�metro {p} em qualquer lugar do template e um
par�metro [nome_da_lista.p] somente dentro da lista de mesmo nome, sem que haja conflitos.
se o par�metro {p} aparecer dentro de uma lista, ser� substitu�do pelo valor correspondente
como em qualquer outro lugar do texto. Os par�metros refecenciados com o nome da lista
tem escopo local e seus valores devem ser atualizados a cada itera��o ou permanecer�o vazios.
Os atributos se aplicam a cada par�metro individualmente e se mantem os mesmos a cada
itera��o.

Exemplo:

{list Cliente}
Nome:       {Cliente.Nome}
Endere�o:   {Cliente.Encereco size=10}

{/list}

Se esta lista tiver dois elementos, a sa�da ser� semelhante a esta:
Nome:       Jos� da Silva
Endere�o:   Rua Joaqui        --> truncado para 10 caracteres

Nome:       Pedro de Oliveira
Endere�o:   Rio de Jan        --> truncado para 10 caracteres

------------------------------------------------

Exemplo de uso:

var
  Template: TStringr;
begin
  Template := TSTringr.Create('template.txt');
  Template['nome'].Valor := 'Evandro';
  Template.OnList = ProcessaLista;
end;

procedure ProcessaLista(sender: TObject; Lista: TListaTemplate; var FimDaLista: boolean);
begin
  if (Lista.Nome = 'clientes') then
  begin
    Lista['nome'].Valor := 'Evandro';
    FimDaLista := True;
  end;
end;