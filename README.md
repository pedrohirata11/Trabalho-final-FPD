/*----------Trabalho de FPD--------------
	    
	Beatriz Kaori Sakai - n° 04 - 71B
  Emily Rocha Sebastião - n° 14 - 71B
	Pedro Kazuki Mori Hirata - n° 28 - 71A  
	Professora: Ariane
	    
  Tema: Loja de ebooks
	ano 2021
----------------------------------------*/

// Bibliotecas
#include <stdio.h>
#include <stdlib.h>
#include <conio.h>
#include <string.h>
#include <windows.h>
#include <locale.h>
//__________________________________________________________________

//Mudando o tamanho da tela para 140,35
text_info vActual = {0, 0, 79, 24, WHITE, WHITE, C80, 35, 140, 1, 1};
//__________________________________________________________________

//ponteiro
FILE *fp;
//__________________________________________________________________

//Protótipos de funções
void textcolor(int);
void textbackground(int);
void cursor (int);
void gotoxy(int, int);
void clreol ();
//__________________________________________________________________

//Estrutura
struct info
{
	int ID; //chave primária 
	char nome[30];
	char nome_usuario[10];
	char email[30];
	struct data
	{
		int dia, mes, ano;
	}data_nascimento;
	char genero_opcoes[25];
	char excluido;
}cadastro;
//__________________________________________________________________

//Protótipos de funções
void menu();                   
void mascara_dados();          
void digitar_dados();           
void dados_salvar();           
void abrir_arquivo();
void design_cadastro();
void design_do_menu();
void design_da_tabela();
int chave_primaria();	
void pesquisa();
void geral_tabela();
void pesq_cod();
void pesq_nome();
void sistem_info();   
//__________________________________________________________________

//Principal
main()
{
	system("TITLE EMYBOOKS");
	setlocale(LC_ALL,"Portuguese");
  textbackground(0);
  system("cls");
	system("mode con:cols=140 lines=35");
	
	textcolor(15);

	gotoxy(44,25);  printf("       .--.           .---.        .-.");
	gotoxy(44,26);  printf("   .---|--|   .-.     | A |  .---. |~|    .--.");
	gotoxy(44,27);  printf(".--|===|Ch|---|_|--.__| S |--|:::| |~|-==-|==|---.");
	gotoxy(44,28);  printf("|%%|NT2|oc|===| |~~|%%| C |--|   |_|~|CATS|  |___|-.");
	gotoxy(44,29);  printf("|  |   |ah|===| |==|  | I |  |:::|=| |    |GB|---|=|");
	gotoxy(44,30);  printf("|  |   |ol|   |_|__|  | I |__|   | | |    |  |___| |");
	gotoxy(44,31);  printf("|~~|===|--|===|~|~~|%%|~~~|--|:::|=|~|----|==|---|=|");
	gotoxy(44,32);  printf("^--^---'--^---^-^--^--^---'--^---^-^-^-==-^--^---^-'");
	
	textcolor(13);
	
  gotoxy(65,17);	printf("B");  Sleep(200);
  gotoxy(66,17);	printf("E");  Sleep(200);
  gotoxy(67,17);  printf("M");  Sleep(200);
  gotoxy(68,17);  printf(" "); 
  gotoxy(69,17);  printf("V");  Sleep(200);
  gotoxy(70,17);  printf("I");  Sleep(200);
  gotoxy(71,17);  printf("N");  Sleep(200);
  gotoxy(72,17);  printf("D");  Sleep(200);
  gotoxy(73,17);  printf("O");  Sleep(200);
  gotoxy(74,17);  printf(" ");  
  gotoxy(75,17);  printf("<3"); Sleep(4000);

	system("cls");

	menu();
}
//__________________________________________________________________

//Funções do menu
void menu() 
{
	int opc;

	while((opc != 1 || opc != 2) || (opc != 3 || opc !=4))
	{
		design_do_menu();
		textcolor(11);
		gotoxy(56,13); printf("1 - Cadastro de usuário");
		gotoxy(56,14); printf("2 - Área de pesquisa de cadastros");
		gotoxy(56,15); printf("3 - Informações do sistema");
		gotoxy(56,16); printf("4 - Sair");
		
		gotoxy(56,20); printf("Digite o opção desejada:");
		gotoxy(80,20); scanf("%d", &opc);
		
		if(opc==1)
		{
			system("cls");
			digitar_dados();
		}
		else if(opc==2)
		{
			system("cls");
			pesquisa();
		}
		else if(opc==3)
		{
			system("cls");
			sistem_info();
		}
		else if(opc==4)
		{
			system("cls");
			break;
		}
		else
		{
			gotoxy(80,21); printf("Opção inválida!!");
			gotoxy(79,20); printf("       "); clreol();
		}
	};
}
//_________________________________________________________________________
void digitar_dados()
{
	abrir_arquivo();
	int variavel;
	do
  {
		design_cadastro();
  	mascara_dados();
  	
  	cadastro.ID=chave_primaria();
  	if (cadastro.ID==0) 
  	{	
			system("cls");
			menu(); 
		}
  	fflush(stdin);
//__________________________________________________________
  	do
		{
  		gotoxy(51,12);	gets(cadastro.nome);
		}while(strlen(cadastro.nome)<3 || strlen(cadastro.nome)>30);
		fflush(stdin);
//__________________________________________________________
    do
		{
			gotoxy(62,13);  gets(cadastro.nome_usuario);
		}while(strlen(cadastro.nome_usuario)<2 || strlen(cadastro.nome_usuario)>10);
		fflush(stdin);
//__________________________________________________________	
		do 
		{
			fflush(stdin);
			gotoxy(51,14);gets(cadastro.email);
			if(strstr(cadastro.email,"@")==0)
			{
				gotoxy(88,14); printf("Email inválido");
				gotoxy(51,14); printf("                                     ");
			}
		}while(strstr(cadastro.email,"@")==0);
		
//Data de nascimento
//_____________________________________________________________________________
		
		do
		{
	  	gotoxy(64,15);	scanf("%d",&cadastro.data_nascimento.dia);
			if(cadastro.data_nascimento.dia<1 && cadastro.data_nascimento.dia>31)
			{
				gotoxy(75,15);	printf("Data inválida!");
	      gotoxy(64,15);  printf("  ");
			}
		}while(cadastro.data_nascimento.dia<1 || cadastro.data_nascimento.dia>31 );
//_____________________________________________________________________________		
		do
		{
			gotoxy(67,15);	scanf("%d",&cadastro.data_nascimento.mes);
			if(cadastro.data_nascimento.mes<1 || cadastro.data_nascimento.mes>12 )
			{
				gotoxy(88,15);	printf("Mês inválido");
				gotoxy(67,15);  printf("  ");
			}	
			else if(cadastro.data_nascimento.mes==2 && cadastro.data_nascimento.dia>29)
			{
				gotoxy(88,15);	printf("Dia do mês inválido");
				gotoxy(67,15);  printf("  ");
			}		
		}while((cadastro.data_nascimento.mes<1 || cadastro.data_nascimento.mes>12) || (cadastro.data_nascimento.mes==2 && cadastro.data_nascimento.dia>29));
		
//________________________________________________________________________________
		do
		{
			gotoxy(70,15);	scanf("%d",&cadastro.data_nascimento.ano);
			if(cadastro.data_nascimento.ano > 2021 || cadastro.data_nascimento.ano <=0)
			{
				gotoxy(88,15);  printf("Ano inválido");
				gotoxy(70,15);  printf("    ");
			}
		}while(cadastro.data_nascimento.ano > 2021 || cadastro.data_nascimento.ano <=0 );
		
//Opções de genero literário
//________________________________________________________________________________		
		do
		{
			fflush(stdin);
			gotoxy(94,16); gets(cadastro.genero_opcoes);
			strlwr(cadastro.genero_opcoes);
			
			if(strstr("suspense/horror",cadastro.genero_opcoes))
			{
				strcpy(cadastro.genero_opcoes,"Suspense/Horror");
				variavel=0;
			}
			else if(strstr("sonetos/poesia",cadastro.genero_opcoes))
			{
				strcpy(cadastro.genero_opcoes,"Sonetos/Poesia");
				variavel=0;
			}
			else if(strstr("acao e aventura",cadastro.genero_opcoes)!=0)
			{
				strcpy(cadastro.genero_opcoes,"Ação e aventura");
				variavel=0;
			}
			else if(strstr("ficcao/ficcao cientifica",cadastro.genero_opcoes)!=0)
			{
				strcpy(cadastro.genero_opcoes,"Ficção/ficção científica");
				variavel=0;
			}
			else if(strstr("romance",cadastro.genero_opcoes)!=0)
			{
				strcpy(cadastro.genero_opcoes,"Romance");
				variavel=0;
			}
			else if(strstr("fantasia",cadastro.genero_opcoes))
			{
				strcpy(cadastro.genero_opcoes,"Fantasia");
				variavel=0;
			}
			else if(strstr("lgbtq+",cadastro.genero_opcoes))
			{
				strcpy(cadastro.genero_opcoes,"LGBTQ+");
				variavel=0;
			}
	    else if(strstr("comedia",cadastro.genero_opcoes))
			{
				strcpy(cadastro.genero_opcoes,"Comédia");
				variavel=0;
			}
			else if(strstr("conto",cadastro.genero_opcoes))
			{
				strcpy(cadastro.genero_opcoes,"Conto");
				variavel=0;
			}
			else if(strstr("infantil",cadastro.genero_opcoes))
			{
				strcpy(cadastro.genero_opcoes,"Infantil");
				variavel=0;
			}
			else if(strstr("hqs/webtoons",cadastro.genero_opcoes))
			{
				strcpy(cadastro.genero_opcoes,"HQs/Webtoons");
				variavel=0;
			}
			else
			{
				variavel=1;
				gotoxy(125,16); printf("Opção inválida");
				gotoxy(93,16); printf("                            ");
			}
		}while (variavel);	
		
//________________________________________________________________________	
		cadastro.excluido='N';

		dados_salvar();
		
//reprocessamento
//________________________________________________________________________
		int reprocessamento;
		
		while(reprocessamento != 1 || reprocessamento != 2)
		{
			gotoxy(50,31); printf("1-Para continuar <3");
			gotoxy(50,32); printf("2-Para voltar para o menu <3");
			fflush(stdin);
			
			gotoxy(50,34); printf("Sua resposta:");
			gotoxy(64,34); scanf("%d",&reprocessamento);
			
			if(reprocessamento==1)
			{
				int x;
				digitar_dados();
			}
			else if(reprocessamento==2)
			{
				system("cls");
				int y;
				menu();
			}
			else
			{
				gotoxy(50,35); printf("Opção inexistente!");
				gotoxy(63,34); clreol();
			}
		};

	}while(true);
	
	
}
//_________________________________________________________________________

void design_do_menu()
{
	textcolor(13);
  textbackground(0);
  system("cls");
  
	gotoxy(55,6); printf(" _ __ ___   ___ _ __  _   _ ");
	gotoxy(55,7); printf("| '_ ` _ %c / _ %c `_ %c| | | |",92,92,92);
	gotoxy(55,8); printf("| | | | | |  __/ | | | |_| |");
	gotoxy(55,9); printf("|_| |_| |_|%c___|_| |_|%c__,_|",92,92);

	gotoxy(20,5); 
	char i;
  for(i=0;i<=49;i++)
  {
  	printf("-+",i);
	}
	
	gotoxy(20,28); 
	char a;
  for(a=0;a<=49;a++)
  {
  	printf("-+",a);
	}
	
	char y;
	for(y=5; y<=28; y++)
	{
		gotoxy(20,y); printf("|",y);	
		gotoxy(120,y); printf("|",y);	
	}

}
//__________________________________________________________________________
void design_cadastro()
{
	system("cls");
	gotoxy(57,5); printf("--------------------");
	gotoxy(57,6); printf("|     CADASTRO     |");
	gotoxy(57,7); printf("--------------------");
	
	gotoxy(95,19); printf("-----------------------------");
	gotoxy(95,20); printf("| Quando digitar a data de  |");
	gotoxy(95,21); printf("| nascimento, dar ENTER!!   |");
	gotoxy(95,22); printf("| Não clicar no TAB!!       |");
	gotoxy(95,23); printf("-----------------------------");
}
//__________________________________________________________________________
void mascara_dados()
{
	gotoxy(45,10); 		printf("ID (para encerrar digite 0): ");
  gotoxy(45,12);    printf("Nome: ");
  gotoxy(45,13);    printf("Nome de usuario: ");
  gotoxy(45,14);    printf("Email:");
  gotoxy(45,15);    printf("Data de nascimento:  /  /    ");
  
  gotoxy(45,16);    printf("Digite seu gênero preferido(disponível na loja): ");
  //menu de opções de gêneros de livros
  gotoxy(47,19); 		printf("%c Suspense/Horror",62);
  gotoxy(47,20);    printf("%c Sonetos/Poesia",62);
  gotoxy(47,21);    printf("%c Ação e aventura",62);
  gotoxy(47,22);    printf("%c Ficção/ficção científica",62);
  gotoxy(47,23);		printf("%c Romance",62);
  gotoxy(47,24);		printf("%c Fantasia",62);
  
  char i;
  for(i=19; i<=24; i++)
  {
  	gotoxy(74,i);		printf("%c",124);
	}
	
  gotoxy(75,19);		printf("%c LGBTQ+",62);
  gotoxy(75,20);		printf("%c Comédia",62);
  gotoxy(75,21);    printf("%c Conto",62);
  gotoxy(75,22);		printf("%c Infantil",62);
  gotoxy(75,23);		printf("%c HQs/Webtoons",62);
  
  char x;
  for(x=46; x<=89; x++)
  {
  	gotoxy(x,18); printf("-");
  	gotoxy(x,25); printf("-");
	}
	
	char y;
	for(y=18; y<=25; y++)
	{
		gotoxy(46,y); printf("|");
		gotoxy(89,y); printf("|");
	}
}
//__________________________________________________________________________
int chave_primaria()
{
	int auxiliar_codigo, F;
	do
	{
		rewind(fp);
		F=0;	

		
		gotoxy(73,10);	scanf("%d",&auxiliar_codigo);
		
		if(auxiliar_codigo==0) 
		{
			break;
		}
		
		while(fread(&cadastro,sizeof(cadastro),1,fp)==1)
		{
			if (cadastro.ID == auxiliar_codigo && cadastro.excluido=='N') 
			{
				F=1;
			}
		}
		
		if(F==1)
		{
			fflush(stdin);
			gotoxy(73,11); 
			printf("ID inválido!!!");
			getch();
			gotoxy(73,11); clreol();
			gotoxy(74,10);printf("          ");
		}
	}while(F==1);
	return auxiliar_codigo;
}
//__________________________________________________________________________
void abrir_arquivo()
{
	if((fp = fopen("arq.ebooksloja.bin", "ab+")) == NULL) 
	{
		gotoxy(55,15);printf("Erro na abertura do arquivo!!");
	  exit(1);
	}	
}
//__________________________________________________________________________
void dados_salvar()
{
	char c;
	
	gotoxy(45,27); printf("Deseja salvar as informações fornacidas? [S]ou[N]");
	fflush(stdin);
	
	while( c != 's' && c != 'S' && c != 'n' && c != 'N' )
	{
		gotoxy(95,27);	printf("  ");
		gotoxy(95,27);	c = getche();
	} 
	
	if(c == 's' || c == 'S')
	{
		if (fwrite(&cadastro, sizeof(cadastro), 1, fp) != 1)
			{
				gotoxy(45,29);printf("Erro na escrita do arquivo");
			}
			else
			{
				fflush(fp);
				gotoxy(45,29);printf("Dados salvos com sucesso!");
				getche();
			}
	}
	else
	{
		gotoxy(45,29); printf("Excluindo as informações fornecidas");
	}
}
//__________________________________________________________________________
void pesquisa()
{
	int a;
	
	do 
	{
		gotoxy(60,5); printf("--------------------");
		gotoxy(60,6); printf("|     PESQUISA     |");
		gotoxy(60,7); printf("--------------------");
		
		gotoxy(55,15); printf("1 - Pesquisa por nome");
		gotoxy(55,16); printf("2 - Pesquisa por código");
		gotoxy(55,17); printf("3 - Tabela geral");
		gotoxy(55,18); printf("4 - Voltar para o menu");
		
		gotoxy(55,20); printf("Sua resposta:");
		gotoxy(68,20); scanf("%d", &a);
		
		if(a == 1)
		{
			system("cls");
			pesq_nome();
		}
		else if(a == 2)
		{
			system("cls");
			pesq_cod();
		}
		else if(a == 3)
		{
			system("cls");
		  geral_tabela();	
		}
		else if(a == 4)
		{
			system("cls");
			menu();
		}
		else
		{
			gotoxy(68,21); printf("Opção inválida");
			gotoxy(68,20); printf("               "); clreol();
		}	
	}while(!((a == 1 || a == 2) || (a == 3 || a == 4)));
	
}
//__________________________________________________________________________
void geral_tabela()
{
	int lin = 5;
	int tecla;
	if((fp = fopen("lojadechinelos.bin", "rb")) == NULL) 
	
	{
	 	system("cls");
	    gotoxy(18,11);printf("Nao ha dados cadastrados");
	    fflush(stdin);
		getch();
		int qw;
		system ("cls");
		menu();
	}
		
	rewind(fp);
	design_da_tabela();
	while( !feof(fp) )
	{
		if(fread(&cadastro, sizeof(cadastro), 1, fp)==1  && cadastro.excluido== 'n')
		{
			
			if(lin>22)
			{
				gotoxy(2,18);printf("Pressione uma tecla para continuar ou ESC para sair...");
				fflush(stdin);
				tecla = getch();
				if(tecla == 27) 
					break;
				lin=5;
			}
			gotoxy(4,lin);printf("%d",cadastro.ID);
			gotoxy(30,lin);printf("%s",cadastro.nome);
			gotoxy(67,lin);printf("%s",cadastro.nome_usuario);
			gotoxy(65,lin);printf("%s",cadastro.email);
			gotoxy(49,lin);printf("%d",cadastro.data_nascimento.dia);
			gotoxy(51,lin);printf("/%d",cadastro.data_nascimento.mes);
			gotoxy(53,lin);printf("/%d",cadastro.data_nascimento.ano);
			gotoxy(72,lin);printf("%s",cadastro.genero_opcoes);
			lin++;
		}		
	}
			

	
	fclose(fp);
	gotoxy(20,25); printf("Tecle ENTER para voltar para o menu");
	getch();
	system("cls");
	
	menu();

}	

//__________________________________________________________________________
void pesq_cod()
{
	int controle = 0;
	int a;
	char c;
	fclose(fp);
	int pesquisa_cod;
	do
	{
		system("cls");
		cursor(1);
		gotoxy(18,5);printf("Digite o código a ser consultado (0 para encerrar): "); 
		scanf("%d",&pesquisa_cod);
		if(pesquisa_cod==0)
		{
			system("cls");
			cursor(0);
			pesquisa();
			return;
		}
		if((fp = fopen("arq.ebooksloja.bin","rb")) == NULL)
		{
			gotoxy(36,18);printf("ERRO NA ABERTURA DO ARQUIVO");
	 		return;
		}
		while(!feof(fp))
		{
			if(fread(&cadastro, sizeof(cadastro), 1, fp) == 1)
			{
				if(pesquisa_cod==cadastro.ID)
				{
					gotoxy(38,2);textcolor(RED);printf("PESQUISA - CÓDIGO");textcolor(BLACK);
					gotoxy(18,9);printf("Código: %d",cadastro.ID); 
					gotoxy(18,11);printf("Nome: %s",cadastro.nome);
					gotoxy(18,13); printf("Nome de usuário: %s", cadastro.nome_usuario);
					gotoxy(18,15); printf("Email: %s",cadastro.email); 
					gotoxy(18,17);printf("Data de Lançamento: %s/%s/%s",cadastro.data_nascimento.dia
																	   ,cadastro.data_nascimento.mes
																	   ,cadastro.data_nascimento.ano);
					gotoxy(18,19);printf("Gênero: %s",cadastro.genero_opcoes);
					gotoxy(18,27);printf("\n\n\n\t\t Deseja continuar a consulta? (S/N): ");
					int confirmacao;
					do
					{
						gotoxy(35,31); confirmacao = getche();
						
					}while(confirmacao != 's' && confirmacao != 'S' && confirmacao != 'n' && confirmacao != 'N');
					
					if(confirmacao == 's' || confirmacao == 'S')
					{
						system("cls");
					}
					if(confirmacao == 'n' || confirmacao == 'N')
					{
						system("cls");
						return; 
					}
				}	
			}
		}
		fclose(fp);
		system("cls");
	}while(TRUE);
}
//__________________________________________________________________________
void pesq_nome()
{
	system("cls");
	char nome_original[40];
	char aux_nome[40];
	int lin = 5;
	int tecla;
	gotoxy(3,2);printf("Digite o nome que deseja pesquisar:");
	scanf("%s",&aux_nome);
	strcpy(nome_original,aux_nome); 
	strupr(aux_nome); 
	if((fp = fopen("lojadechinelos.bin", "rb")) == NULL) 
	{
	  gotoxy(18,11);printf("Nao ha dados cadastrados");
	  fflush(stdin);
		getch();
		int qw;
		system("cls");
		menu();
	}


  if(strstr(aux_nome,cadastro.nome ) == NULL)
	{
		gotoxy(2,lin);printf("Nao ha o respectivo registro.");
		gotoxy(20,25); printf("\nTecle ENTER para voltar para o menu");
		getch();
		system("cls");

		int tr;
		menu();
				
	}	
	strupr(cadastro.nome); 
	
	if(strstr(aux_nome,cadastro.nome) != NULL)
	{			
		design_da_tabela();
		while( !feof(fp) )
		{
			if(fread(&cadastro, sizeof(cadastro), 1, fp) == 1 && cadastro.excluido== 'n')
			{
				if(lin>22)
				{
					gotoxy(2,18);printf("Pressione uma tecla para continuar ou ESC para sair...");
					fflush(stdin);
					tecla = getch();
					if(tecla == 27) 
					break;
					lin=5;
				}
				gotoxy(4,lin);printf("%d",cadastro.ID);
				gotoxy(30,lin);printf("%s",cadastro.nome);
				gotoxy(65,lin);printf("%s",cadastro.nome_usuario);
				gotoxy(67,lin);printf("%s",cadastro.email);
				gotoxy(49,lin);printf("%d",cadastro.data_nascimento.dia);
				gotoxy(51,lin);printf("/%d",cadastro.data_nascimento.mes);
				gotoxy(53,lin);printf("/%d",cadastro.data_nascimento.ano);
				gotoxy(67,lin);printf("%s",cadastro.genero_opcoes);
				lin++;
		  }		
  	}
		fclose(fp);
		
		gotoxy(20,25); printf("Tecle ENTER para voltar para o menu");
		getch();
		system("cls");
	
		int tr;
		menu();
	}
}
//__________________________________________________________________________
// info do sistema

void sistem_info()
{
	textcolor(BLACK);
	textcolor(RED);
  textbackground(WHITE);
  system("cls");
    gotoxy(56,5); printf("----------------------------");
    gotoxy(56,6); printf("|      INFO DO SISTEMA     |");
    gotoxy(56,7); printf("----------------------------");
    
    gotoxy(50,10);   printf("Nome da empresa:");
    gotoxy(50,11);   printf(">> EMYBOOKS COMPANY <<"); 
    
    gotoxy(50,13);   printf("Integrantes:    Beatriz Kaori Sakai - n° 04       71B");
    gotoxy(66,14);                   printf("Emily Rocha Sebastião - n° 14     71B");
    gotoxy(66,15);                   printf("Pedro Kazuki Mori Hirata - n° 28  71A");    
    
    gotoxy(49,17);   printf("Nosso software trata-se sobre uma simulação");
    gotoxy(49,18);   printf("de um sistema onde o usuário cria uma conta");
    gotoxy(49,19);   printf("para se tornar cliente da loja. ");
    gotoxy(49,20);   printf("O sistema é composto por um cadastro(código,");
    gotoxy(49,21);   printf("nome, nome de usuário, email, data e gênero");
    gotoxy(49,22);   printf("preferido), área de pesquisa, onde possui a ");
    gotoxy(49,23);   printf("tebela geral, consulta por nome e código, e ");
    gotoxy(49,24);   printf("info do software.");
    
    gotoxy(50,26);   printf("Agradecimentos:");
    gotoxy(49,27);   printf("Gostaríamos de agradecer a professora Ariane");
    gotoxy(49,28);   printf("Scarelli pela maravilhosa orientação e auxílio");  
		gotoxy(49,29);   printf("para o desenvolvimento do respectivo sistema.");
		
		gotoxy(50,31); printf("Pressione qualquer tecla para retornar ao menu");
		getch();
		system("cls");
}
//__________________________________________________________________________
void design_da_tabela()
{
	textcolor(BLACK);
	textcolor(RED);
  textbackground(WHITE);
  system("cls");
  system("mode con:cols=140 lines=35");
	
	gotoxy (15,5);printf("__________________________________________________________________________________________________________________");
	gotoxy (15,6);printf("|ID      |Nome            |Nome de usuário |Email                 |Data       |Gênero preferido         |Excluido |");
	gotoxy (15,7);printf("|--------|----------------|----------------|----------------------|-----------|-------------------------|---------|");
	gotoxy (15,8);printf("|        |                |                |                      |           |                         |         |");
	gotoxy (15,9);printf("|--------|----------------|----------------|----------------------|-----------|-------------------------|---------|");
	gotoxy(15,10);printf("|        |                |                |                      |           |                         |         |");
	gotoxy(15,11);printf("|--------|----------------|----------------|----------------------|-----------|-------------------------|---------|");
	gotoxy(15,12);printf("|        |                |                |                      |           |                         |         |");
	gotoxy(15,13);printf("|--------|----------------|----------------|----------------------|-----------|-------------------------|---------|");
	gotoxy(15,14);printf("|        |                |                |                      |           |                         |         |");
	gotoxy(15,15);printf("|--------|----------------|----------------|----------------------|-----------|-------------------------|---------|");
	gotoxy(15,16);printf("|        |                |                |                      |           |                         |         |");
	gotoxy(15,17);printf("|--------|----------------|----------------|----------------------|-----------|-------------------------|---------|");
	gotoxy(15,18);printf("|        |                |                |                      |           |                         |         |");
	gotoxy(15,19);printf("|--------|----------------|----------------|----------------------|-----------|-------------------------|---------|");
	gotoxy(15,20);printf("|        |                |                |                      |           |                         |         |");
	gotoxy(15,21);printf("|--------|----------------|----------------|----------------------|-----------|-------------------------|---------|");
	gotoxy(15,22);printf("|        |                |                |                      |           |                         |         |");
	gotoxy(15,23);printf("|--------|----------------|----------------|----------------------|-----------|-------------------------|---------|");
	gotoxy(15,24);printf("|        |                |                |                      |           |                         |         |");
	gotoxy(15,25);printf("|________|________________|________________|______________________|___________|_________________________|_________|");
}
//__________________________________________________________________________

//Funções											       
void textcolor(int newcolor)
{
   CONSOLE_SCREEN_BUFFER_INFO csbi;

   GetConsoleScreenBufferInfo(GetStdHandle(STD_OUTPUT_HANDLE), &csbi);
   SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), 
      (csbi.wAttributes & 0xf0) | newcolor);
   vActual.attribute = (csbi.wAttributes & 0xf0) | newcolor;
}
//___________________________________________________________________

//___________________________________________________________________
void textbackground(int newcolor)
{
   CONSOLE_SCREEN_BUFFER_INFO csbi;

   GetConsoleScreenBufferInfo(GetStdHandle(STD_OUTPUT_HANDLE), &csbi);
   SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), 
      (csbi.wAttributes & 0x0f) | (newcolor << 4));
   vActual.attribute = (csbi.wAttributes & 0x0f) | (newcolor << 4);
}
//___________________________________________________________________
void cursor (int x) { 
	switch (x) {
		case 0: {//ocultar cursor
			CONSOLE_CURSOR_INFO cursor = {1, FALSE};
			SetConsoleCursorInfo(GetStdHandle(STD_OUTPUT_HANDLE), &cursor);
			break;
		}
		case 1: {//apresentar cursor
			CONSOLE_CURSOR_INFO cursor = {1, TRUE};
			SetConsoleCursorInfo(GetStdHandle(STD_OUTPUT_HANDLE), &cursor);
			break;
		}
	}
}
//___________________________________________________________________
void gotoxy(int x, int y)
{
  COORD c;
  c.X = x - 1;
  c.Y = y - 1;
  SetConsoleCursorPosition (GetStdHandle(STD_OUTPUT_HANDLE), c);
}
//___________________________________________________________________
void clreol ()
{
   COORD coord;
   DWORD escrito;

   coord.X = vActual.winleft+vActual.curx-1;
   coord.Y = vActual.wintop+vActual.cury-1;
   
   FillConsoleOutputCharacter(GetStdHandle(STD_OUTPUT_HANDLE), ' ', 
      vActual.screenwidth - vActual.curx + 1, coord, &escrito);
   FillConsoleOutputAttribute(GetStdHandle(STD_OUTPUT_HANDLE),
      vActual.attribute, vActual.screenwidth - vActual.curx + 1, 
      coord, &escrito);
   gotoxy(vActual.curx, vActual.cury);
}

