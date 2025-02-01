```markdown
# Projeto de Geração de Imagens com Azure OpenAI

Este projeto demonstra como usar a API do Azure OpenAI para gerar imagens usando o modelo DALL-E 3 e exibi-las.

## Descrição

Este projeto utiliza a biblioteca `httpx` para fazer requisições HTTP, `azure.identity` para autenticação com o Azure, `openai` para interagir com a API do Azure OpenAI, e `PIL` para manipulação de imagens.

## Instalação

Siga os passos abaixo para configurar o ambiente e instalar as dependências necessárias:

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/seu-repositorio.git

# Navegue até o diretório do projeto
cd seu-repositorio

# Crie um ambiente virtual
python -m venv venv

# Ative o ambiente virtual
# No Windows
venv\Scripts\activate
# No macOS/Linux
source venv/bin/activate

# Instale as dependências
pip install -r requirements.txt
```

## Uso

Aqui está um exemplo de como usar o código Python fornecido:

```python
import os
import httpx
from azure.identity import DefaultAzureCredential
from openai import AzureOpenAI
from PIL import Image
import json

credential = DefaultAzureCredential()
client = AzureOpenAI(
    api_version="2024-02-01",
    azure_endpoint=os.environ['AZURE_OPENAI_ENDPOINT'],
    credential=credential
)

result = client.images.generate(
    model="dalle3", # the name of your DALL-E 3 deployment
    prompt="O nome 'Karlos Claro' escrita em um banner de cidade, exibido em uma rua com vários carros passando",
    n=1
)

# Set the directory for the stored image
image_dir = os.path.join(os.curdir, 'images')

# If the directory doesn't exist, create it
if not os.path.isdir(image_dir):
    os.mkdir(image_dir)

# Initialize the image path (note the filetype should be png)
image_path = os.path.join(image_dir, 'generated_image.png')

# Retrieve the generated image
json_response = json.loads(result.model_dump_json())
image_url = json_response["data"][0]["url"]  # extract image URL from response
generated_image = httpx.get(image_url).content  # download the image

with open(image_path, "wb") as image_file:
    image_file.write(generated_image)

# Display the image in the default image viewer
image = Image.open(image_path)
image.show()
```

## Contribuição

Se você deseja contribuir para este projeto, siga os passos abaixo:

1. Faça um fork do projeto
2. Crie uma nova branch (`git checkout -b feature/nova-feature`)
3. Commit suas mudanças (`git commit -m 'Adiciona nova feature'`)
4. Faça um push para a branch (`git push origin feature/nova-feature`)
5. Abra um Pull Request

## Licença

Este projeto está licenciado sob a Licença MIT - veja o arquivo LICENSE para mais detalhes.
```