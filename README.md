# Criação de Vídeo Automático com IA

![Fluxo Principal - Criação de Vídeo Auto com IA](./CRIA%C3%87%C3%83O%20DE%20VIDEO%20AUTO%20COM%20IA.png)

Este projeto demonstra um fluxo **100% automatizado** para criar vídeos usando **IA**. Todo o processo — desde a geração de roteiro, narração, criação de imagens e montagem final do vídeo — é orquestrado no **n8n** com diversos agentes de IA. O objetivo é produzir conteúdo de forma rápida e escalável para postar em plataformas online (YouTube, TikTok, Instagram, etc.).

---

## O que o Fluxo Faz?

1. **Geração de Roteiro com IA**  
   - Recebe a **ideia** ou **tópico** do vídeo (ex.: “a história de Esparta”) e gera um **roteiro de 60 segundos** usando um modelo de linguagem (OpenAI GPT).  
   - Utiliza prompts específicos para escrever o texto de forma atrativa, usando letras maiúsculas e pontos de exclamação.

2. **Conversão do Roteiro em Narração**  
   - Formata o texto (removendo quebras e cenas) para que fique apenas a narração.  
   - Envia o texto para um serviço de **Text-to-Speech** (ex.: ElevenLabs) que gera o **áudio**.  
   - Faz o **upload** desse áudio em uma pasta do Google Drive, obtendo o link público.

3. **Transcrição de Áudio (opcional)**  
   - Caso já haja um áudio externo (ou para reaproveitar), o fluxo também pode transcrever usando **OpenAI Whisper**, convertendo arquivo binário em texto.  
   - Integra com o n8n para processar e juntar as informações.

4. **Geração de Imagens (Cenas) com IA**  
   - A partir do roteiro (ou narração), o fluxo gera **prompts** para cada cena, definindo **start_time**, **end_time** e **prompt** de imagem.  
   - Envia esses prompts para um modelo de imagem (Leonardo ou outro).  
   - Aguarda um tempo (Wait) para que o serviço conclua a geração e, em seguida, obtém o link das imagens.

5. **Criação de Animações/“Motion”**  
   - Para cada imagem gerada, o fluxo solicita ao serviço (Leonardo) a criação de um “motion” (pequena animação/MP4).  
   - Faz polling para saber quando está pronto e então baixa os arquivos de vídeo correspondentes.

6. **Montagem Final do Vídeo**  
   - Usa um serviço de edição (Shotstack, por exemplo) para juntar todas as animações, seguindo os tempos (start/end) definidos.  
   - Aplica o áudio de narração como trilha sonora (soundtrack).  
   - Espera o serviço renderizar e faz polling até que o vídeo final esteja disponível.  
   - Baixa o vídeo renderizado automaticamente.

7. **Fluxo 100% Automatizado**  
   - Ao final, você tem um vídeo completo de 1 minuto, **com cenas geradas por IA**, narração de IA e montado sem intervenção manual.  
   - Tudo pronto para subir nas redes sociais!

---

## Principais Componentes

- **n8n**: Orquestra toda a automação e integra com serviços externos.  
- **OpenAI GPT**: Gera o roteiro (texto) e prompts intermediários.  
- **ElevenLabs** (ou outro TTS): Converte o roteiro em narração de áudio.  
- **Google Drive**: Armazena e serve o arquivo de áudio gerado.  
- **Leonardo**: Gera imagens e animações (motion) a partir de prompts.  
- **Shotstack**: Edita e renderiza o vídeo final.  
- **OpenAI Whisper** (opcional): Transcreve áudios, caso necessário.

---

## Como Funciona o Fluxo (Visão Simplificada)

1. **Definição do Tópico**  
   Você insere um tópico ou ideia do vídeo (ex.: “The history of Sparta”).

2. **Geração de Script**  
   - Um prompt “60 Second Script Writer” cria um roteiro de 60 segundos, com entonação empolgante.

3. **Formatação e Narração**  
   - O script é formatado para remoção de instruções de cena.  
   - O texto segue para o TTS (Text-to-Speech) gerando um áudio MP3.

4. **Upload do Áudio**  
   - O MP3 é enviado ao Google Drive, gerando link público.  
   - (Opcional) Se já existe um áudio, ele pode ser transcrito via Whisper.

5. **Geração de Cenas (Imagens)**  
   - A narração é analisada para criar prompts de imagem.  
   - Cada prompt é enviado ao serviço de IA (Leonardo), que retorna imagens.

6. **Criação de Animações**  
   - As imagens são convertidas em pequenos vídeos (motion) usando a API do Leonardo.  
   - O fluxo aguarda (Wait) até estarem prontos e baixa cada MP4.

7. **Edição com Shotstack**  
   - O fluxo cria uma timeline definindo “start” e “length” de cada cena.  
   - O áudio (narração) é aplicado como trilha sonora.  
   - Shotstack renderiza o vídeo final e retorna uma URL para download.

8. **Resultado**  
   - O fluxo baixa o arquivo MP4 final.  
   - Você obtém um vídeo completo, 100% criado por IA (imagens + narração).

---

