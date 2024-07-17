# Debugando Localmente AWS Lambda no VS Code

## Introdução

Olá. tudo bem?
Aqui irei abordar sobre como debugar função Lambda da AWS localmente usando o VS Code. Todas as Clouds disponibilizam um editor para o desenvolvimento das funções serveless. No entanto, com uma série de limitações que não encontramos quando estamos desenvolvendo em uma IDE local. E uma das coisas que sinto mais falta é a possibilidade de efetuar um bom DEBUG.
Após pesquisar nas documentações da AWS, em blogs e no Youtube, percebi que este conteúdo é muito pouco e ralo. Eu arriscaria dizer que 95% dos tutoriais são demonstrações usando o AWS Management Console. Assim, resolvi dar uma contribuição fazendo este tutorial para aqueles que desejam debugar no VS Code e está encontrando dificuldades. Vamos lá?

Escreverei o passo a passo para que um maior número de pessoas possa acompanhar mas você pode pular para o que interessar. ;)
<br>
:::info
O meu VS Code estará em inglês. Assim, reaquede os comandos para o idioma do seu.
:::

:::warning
Gostaria de ter feito um único post abordando o debug e deploy. No entanto, este já ficou mais longo do que imaginava e, assim, terá um outro tratando apenas do deploy.
:::

## Artigos:
- [x] Debugando Localmente AWS Lambda no VS Code
- [ ] Deploy da Função Lambda Local (com e sem Layer) para AWS

***

## Pré-requisitos

- [Docker](https://www.docker.com)
- [VS Code](https://code.visualstudio.com/download) :joy:
- [AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html)
- Plugin [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
- e uma função lambda

    <details>
    <summary>Você pode usar esta função, se quiser</summary>

    ```python
    import json


    def lambda_handler(event, context):
        result = {"result": "Lambda executada!" }

        return {
            "statusCode": 200,
            "body": json.dumps(result)
        }
    ```
    </details>

***

## Configuração do Ambiente

- É fortemente recomendado o uso de um ambiente virtual. Nós usaremos o _venv_
```
> python -m venv .venv
```

Vamos abrir o VS Code, selecionar o ambiente virtual e colar a função lambda do exemplo.


> [!TIP]
> Para ativar o interpretador Python do ambiente virtual no VS Code use a seguinte combinação de teclas: ==**_Ctrl + Shift + P_**== (ou pelo menu **_View, Command Pallete..._**) e digite: ==**_>Python: Select Interpreter_**== e selecione o interpretador Python do ambiente virtual criado.

Nosso VS Code deve ficar mais ou menos assim:
![Como deve ficar o VS Code](/assets/img/2024-07-14/fig01_interface_vscode_configurada.png)
*Fig. 1: Interface do VS Code configurada.*


## Vamos Debugar!

## Debugando sem o uso do plugin

Para debugarmos a função lambda sem o plugin adequado, temos que tratá-la como uma função e acrescentar um _main_, conforme código abaixo:

```python
    if __name__ == "__main__":
        event = json.dumps({"titulo":"Debugando Lambda"})
        print(lambda_handler(event=event, context=None))
```

E, então, é só prosseguirmos com o processo de debug:

![Como deve ficar o VS Code](/assets/img/2024-07-14/fig02_1o_exemplo_debug.png)
*Fig. 2: Debugando AWS Lambda como função*

Este método pode ser o mais simples. No entanto, deixa-nos às margens de algumas funcionalidades que do AWS Toolkit. Assim, vamos para o debug que nos interessa! :sunglasses:

## Debugando usando o plugin

Como queremos a integração do VS Code com a AWS, precisamos instalar o **AWS SAM CLI** e o plugin **AWS Toolkit**. Isto porque, ao debugar, será criado um container Amazon Linux que rodará a nossa função lambda.

### Instalando o AWS SAM CLI

Siga as instruções deste [link](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html) para instalar e configurar o **SAM CLI**.


### Instalando e configurando o plugin

O plugin para VS Code pode ser baixado neste [link](https://marketplace.visualstudio.com/items?itemName=AmazonWebServices.aws-toolkit-vscode) ou diretamente no VS Code procurando por **AWS Toolkit**.

> [!TIP]
> Para instruções de instalação e configuração do plugin, acessar este [link](https://docs.aws.amazon.com/toolkit-for-visual-studio/latest/user-guide/setup.html).

Após configurarmos o plugin, seu profile deve ser exibido na parte inferior esqueda do editor:
![AWS Profile](/assets/img/2024-07-14/fig03_awstoolkit_profile.png
)
<br>
*Fig. 3: Profile AWS

### Toolkit: instalado e configurando

Clique no logo da AWS que aparece na barra de atividades e você verá os recursos disponíveis na  AWS
<br>
![Como deve ficar o VS Code](/assets/img/2024-07-14/fig04_apresentacao_awstoolkit.png)
<br>
*Fig. 4: Debugando AWS Lambda como função*

==Com este plugin também é possível fazer algumas coisas legais. Mas vamos focar no debug!==

> [!TIP]
> PSe você já tiver a função lambda na AWS é possível fazer o download dela pelo plugin!
> ![AWS Profile](/assets/img/2024-07-14/fig05_menu_awstoolkit.png)
> <br>
> *Fig. 5: Download/Upload

### Criando a configuração para debugar

Com a nossa função aberta, vamos pressionar ==**_Ctrl + Shift + P_**== e digitar: ==**>sam debug**== e selecionar ==**AWS: SAM Debug Configuration**==

![Como deve ficar o VS Code](/assets/img/2024-07-14/fig06_menu_samdebug.png)
<br>
*Fig. 6: Selecionando AWS: SAM Debug Configuration*
<br>
Isso abrirá uma caixa de seleção. No passo (1/2), selecione o nome da sua função.
No passo (2/2), selecione a versão do seu interpretador Python.

![Como deve ficar o VS Code](/assets/img/2024-07-14/fig07_menu_sandebug_passo1.png)
<br>
*Fig. 7: SAM Debug Configuration: Passo 1/2*
<br>

![Como deve ficar o VS Code](/assets/img/2024-07-14/fig08_menu_sandebug_passo2.png)
<br>
*Fig. 8: SAM Debug Configuration: Passo 2/2*
<br><br>
Após isso, será criado um arquivo **_launch.json_** na pasta **_.vscode_** do nosso projeto e seu conteúdo dever ser bem próximo de:

```json
{
    "configurations": [
        {
            "type": "aws-sam",
            "request": "direct-invoke",
            "name": "lambdaTestNew:lambda_function.lambda_handler (python3.10)",
            "invokeTarget": {
                "target": "code",
                "projectRoot": "${workspaceFolder}/",
                "lambdaHandler": "lambda_function.lambda_handler"
            },
            "lambda": {
                "runtime": "python3.10",
                "payload": {},
                "environmentVariables": {}
            }
        }
    ]
}
```

Assim, para iniciarmos a debugar nossa função basta pressionar **F5**. Caso nós tenhamos outras configurações de debug, podemos clicar em **Debug** e selecionar o nome que demos para a nossa configuração de debug do lambda.

![Como deve ficar o VS Code](/assets/img/2024-07-14/fig09_conf_debug.png)
<br>
*Fig. 9: Launch*

<br>
O AWS SAM irá baixar a imagem para "hospedar" a nossa função lambda e fazer o debug remotamente a partir do nosso VS Code. Ou seja, essa parte do container deve ser transparente para nós.

Ao iniciar o nosso debug, poderemos ver algo assim:
![Como deve ficar o VS Code](/assets/img/2024-07-14/fig10_awssam_dubugging.png)
<br>
*Fig. 10: Debugando com AWS SAM e Toolkit*

Note que ela tem algumas diferenças da [Figura 2](#debugando-sem-o-uso-do-plugin) quando debugamos sem o plugin. Temos **_Context_**. Assim, nossa execução local está exatamente como se ela estivesse sendo executada no ambiente AWS (lembra do container?).


<br>

## Referências:

[Baixar o AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html)

[Baixar o kit de ferramentas para Visual Studio](https://docs.aws.amazon.com/pt_br/toolkit-for-visual-studio/latest/user-guide/downloads.html)

[Installing and setting up the AWS Toolkit for Visual Studio](https://docs.aws.amazon.com/toolkit-for-visual-studio/latest/user-guide/setup.html)

[Launch Additional Configurations](https://code.visualstudio.com/docs/python/debugging#_additional-configurations)

[AWS Toolkit Issues](https://github.com/aws/aws-toolkit-visual-studio/issues)