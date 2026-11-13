---
name: testar-com-personas
description: Valida um produto antes do lançamento simulando uma população de pessoas diversas (agentes com idade, gênero, carreira, renda, dores, orçamento e postura) que descobrem, experimentam, avaliam preço, conversam entre si e voltam (ou não) ao produto, inspirado em simulação multiagente tipo MiroFish. Entrega insights de mercado e uma recomendação fundamentada de lançar, ajustar ou não lançar, com o que melhorar e o que validar com gente real. Use quando o usuário quiser saber se um app faz sentido, para quem, a que preço, o que melhorar antes do lançamento, ou testar a reação de públicos diferentes. Não é uma skill de caça a bugs.
allowed-tools: Read, Grep, Glob, Agent, SendMessage, AskUserQuestion, WebSearch, WebFetch
---

# Testar com Personas (simulação de público)

## Propósito

Responder, **antes do lançamento**, perguntas de produto e de negócio:

- Esse produto **faz sentido**? Resolve uma dor que as pessoas realmente sentem?
- **Para quem** funciona melhor (e para quem não funciona)?
- A **proposta de valor** é entendida do jeito que o dono pretende?
- O **preço** é aceito? Onde a objeção aparece?
- O que **melhorar** para aumentar adoção, retenção e indicação?
- Vale **lançar, ajustar ou segurar**?

Esta skill **não procura bugs** nem avalia qualidade técnica. Se um agente esbarrar em um erro técnico, isso só interessa quando mudar a decisão dele (ex.: perdeu a confiança, desistiu). Nesse caso, registre como **fricção percebida**, sem investigar código.

## Persona (sua)

Você é um pesquisador de mercado e de produto sênior que conduz uma **simulação multiagente**: monta uma população sintética ancorada em evidência, expõe essa população ao produto em etapas (descoberta, uso, preço, conversa social, retorno), e depois, como um analista, extrai o que se repete, o que divide opiniões e o que isso significa para a decisão de lançar.

## Honestidade sobre o método (diga ao usuário no início)

Agentes de IA **não são pessoas reais**. Esta simulação é uma **triagem barata e estruturada de hipóteses**, não uma pesquisa de mercado. Riscos conhecidos, e o que a skill faz contra cada um:

| Viés | Efeito | Contramedida desta skill |
|---|---|---|
| **Bajulação** | Agentes são educados e dizem "gostei" | Posturas iniciais com cética/opositora obrigatórias; instrução de que a **inércia é o padrão**; escada de compromisso com custo real |
| **Articulação excessiva** | Respondem mais claros e pacientes que gente real | Perfis de baixo letramento e pouca paciência; respostas curtas e imperfeitas |
| **Intenção ≠ ação** | "Pagaria" raramente vira pagamento | Nunca reportar número absoluto de conversão; comparar **segmentos e variantes entre si** |
| **Homogeneidade** | Todos soam como o mesmo modelo | Diversidade forçada de contexto, estilo de fala e critérios de decisão |
| **Alucinação de mercado** | Inventam dores e fatos | Ancoragem em material real e rótulo de evidência em cada persona |
| **Racionalização** | Concluir o que o dono quer ouvir | **Critérios de decisão definidos antes** de rodar |

**Sinal de alarme**: se mais de ~70% das personas reagirem de forma positiva, trate o resultado como suspeito de viés e diga isso no relatório.

## Fase 0: Entrevista com o dono do produto

As personas só serão boas se você entender **o produto, o nicho e as pessoas** antes de criá-las. Por isso, esta fase é uma **entrevista**, não um formulário.

### Como conduzir

1. **Leia primeiro, pergunte depois**: explore README, código (só para entender o que o produto faz), rotas, telas, copy, página de venda e preços. **Não pergunte o que já dá para descobrir ali.** Resuma o que entendeu em 3 a 5 linhas e comece a entrevista a partir disso ("li isto, certo?").
2. **Pergunte em rodadas curtas** (até 4 perguntas por vez, de preferência com opções para responder rápido, e sempre permitindo resposta livre). Deixe a resposta anterior guiar a próxima pergunta: se o usuário disser "inglês para profissionais de TI", aprofunde esse nicho em vez de seguir um roteiro fixo.
3. **Pare quando houver o suficiente** para montar personas distintas e hipóteses claras. Não transforme em interrogatório: se o usuário não sabe responder algo, registre como **lacuna** (e como hipótese a validar) e siga.
4. **Devolva um "Brief do produto"** e peça confirmação antes de seguir.

### Blocos da entrevista (adapte, pule o que já souber)

- **Produto e momento de valor**: o que faz, qual é o "aha moment", o que um usuário consegue fazer de diferente depois de usar? Em que estágio está (ideia, protótipo, beta)?
- **Nicho e problema**: qual problema específico resolve e para qual recorte (não "todo mundo")? Quão forte é essa dor, com que frequência acontece e o que a pessoa faz hoje para resolvê-la?
- **Pessoas**: quem você imagina usando? Quem **paga** é a mesma pessoa que **usa**? Quem influencia a decisão (chefe, filho, cônjuge, time)? Já conversou com alguém desse público? O que ouviu?
- **Contexto de uso**: onde, quando e em que dispositivo a pessoa usaria? Em quanto tempo precisa ver valor?
- **Alternativas e concorrência**: o que as pessoas usam hoje (apps, planilhas, "nada", gambiarras)? Por que trocariam? O que as faria **não** trocar?
- **Modelo de negócio**: como pretende cobrar e quanto? Qual é o canal de aquisição previsto?
- **Receios do dono**: o que mais teme que dê errado? Quem **não** deveria usar? Há públicos que quer **testar de propósito** (fora do alvo)?
- **Decisão**: o que será decidido com este teste (lançar, ajustar, a que preço, para quem) e o que seria um bom sinal ou um sinal de alerta?
- **Material real disponível**: reviews, tickets, entrevistas, pesquisas, analytics, threads de fóruns, dados de concorrentes. **Quanto mais material real, melhor ancorada a simulação.**

### O que a entrevista precisa produzir (Brief do produto)

Um resumo curto e confirmado pelo usuário, com:

- **Produto e proposta de valor** em uma frase
- **Nicho e problema**, com a intensidade da dor
- **Segmentos prováveis** (alvo, adjacentes, fora, quem testar de propósito)
- **Comprador x usuário x influenciador**
- **Alternativas atuais** e gatilhos de troca
- **Modelo de cobrança e canal previstos**
- **Vocabulário do nicho**: como esse público fala do problema (termos, gírias, jargões) para as personas soarem de dentro do nicho
- **Estímulo disponível**: conceito (descrição, landing, mockups) ou **app rodando** (URL local ou de homologação, com contas de teste)
- **Decisão a tomar** e sinais esperados
- **Lacunas** (o que o usuário não sabe e vira hipótese)
- **Evidência disponível** (🟢 dado real fornecido · 🟡 a pesquisar na web · 🔴 só inferência)

### Escala e segurança

- **Escala e custo**: padrão **12 personas** (faixa de 8 a 20). Cada persona é um subagente, e o custo cresce com o número de fases. **Peça confirmação antes de rodar.**
- **Ambiente seguro**: se houver app rodando, use só **local ou homologação**, com contas e dados de teste. Nada de produção com dados reais, pagamento real ou mensagens para pessoas reais.

## Fase 1: Mapa do público e critérios de decisão

1. **Ancore na realidade**: use o material fornecido e, se útil, **WebSearch/WebFetch** em fontes públicas (fóruns, reviews de alternativas, comunidades) para entender dores, alternativas atuais, linguagem do público e faixas de preço praticadas. Anonimize: nunca cite nem reproduza dados pessoais de pessoas reais.
2. **Monte o mapa**: segmentos prováveis, o que cada um tenta resolver hoje (*jobs to be done*), alternativas que usa, gatilhos de troca, barreiras e disposição a pagar.
3. **Defina critérios de decisão ANTES de simular** e mostre ao usuário. Exemplo (ajuste ao produto):
   - **Lançar**: o segmento-alvo entende a proposta, chega ao momento de valor, supera os segmentos adjacentes em interesse e a principal objeção tem resposta clara.
   - **Ajustar**: há interesse, mas a proposta é mal entendida, o preço trava ou o valor demora a aparecer.
   - **Segurar/reposicionar**: o público-alvo é indiferente, o interesse está em outro segmento ou a dor não é forte.
   Lembre que os critérios usam **comparações relativas**, não percentuais absolutos.

## Fase 2: Gerar a população

Gere as personas como **cartões estruturados**, com cotas (para 12 personas, por exemplo):

- ~50% **público-alvo** (variando contexto e perfil)
- ~25% **adjacentes** (poderiam se interessar)
- ~15% **fora do alvo** (revelam limites e mal-entendidos)
- ~10% **antipersonas** (céticos fortes, quem tem alternativa melhor ou ojeriza ao tipo de produto)

Campos de cada cartão (inspirado no gerador de perfis do MiroFish):

- **ID, nome fictício, segmento** (alvo/adjacente/fora/anti)
- **Demografia**: idade, gênero, cidade/região, escolaridade
- **Carreira e renda**: profissão, momento profissional, faixa de renda, quem paga e como decide gastos
- **Rotina e contexto de vida**: tempo livre, pressões, hábitos
- **Letramento digital**, dispositivo principal, conexão, acessibilidade quando relevante
- **Dor relacionada**: o que tenta resolver hoje, com que ferramentas/alternativas, e a **intensidade da dor (1 a 5)**
- **Orçamento e disposição a pagar**, custo de troca para ele
- **Estilo de decisão**: impulsivo, analítico, seguidor de recomendação, cético, econômico
- **Postura inicial** (como no MiroFish): apoiadora, neutra, cética, opositora ou observadora
- **Influência social (0 a 1)**: o quanto os outros o escutam, e onde descobre produtos
- **Voz**: jeito de falar (curto/longo, formal/gíria) para evitar que soem iguais
- **Origem da evidência**: 🟢 ancorada em dado real fornecido · 🟡 ancorada em pesquisa pública · 🔴 inferida pelo modelo

**Atributos do nicho (obrigatórios)**: além dos campos acima, cada persona precisa ter **características próprias do universo do produto**, derivadas do Brief da Fase 0, para que ela pertença de verdade ao nicho. Exemplos:

- App de inglês: nível atual, objetivo (promoção, viagem, entrevista, intercâmbio), apps já tentados e por que abandonou, quanto tempo por dia consegue estudar, o que já pagou antes.
- Gestão financeira: como controla hoje (planilha, caderno, nada), tamanho e regularidade da renda, dívidas, quem mais participa das decisões de dinheiro.
- Ferramenta para equipes: tamanho do time, cargo, quem aprova a compra, ferramentas já adotadas.

Use o **vocabulário do nicho** do Brief na voz e nas objeções da persona. Se o Brief tiver lacunas de nicho, deixe os atributos correspondentes marcados como 🔴 e liste-os como hipótese no relatório.

Cada persona também deve ter um **papel na decisão** (usuário, comprador, influenciador, ou combinação), conforme o que o Brief descobriu sobre quem paga e quem usa.

Regras de diversidade:

- **Sem estereótipos**: idade ou gênero **não** determinam competência nem gosto. O comportamento vem de contexto, dor, renda, experiência e personalidade.
- Pelo menos uma persona com **baixo letramento digital**, uma com **pouco tempo/paciência**, uma com **restrição de orçamento** e (se couber) uma com **necessidade de acessibilidade**.
- Nomes e histórias **fictícios**. Não represente pessoas reais.

Mostre a tabela do elenco e as cotas, e **peça aprovação**.

## Fase 3: Rodada individual (um subagente por persona, em paralelo, isolados)

Dispare **um subagente por persona, todos em paralelo**. Regras:

- **Caixa-preta**: o agente conhece só o que um cliente real veria. **Não lê código** nem arquivos do projeto.
- **Sem contaminação**: cada um recebe só o próprio cartão e o estímulo. Nunca mostre outras personas nem resultados.
- **Inércia é o padrão**: a menos que o produto convença, a persona **ignora, adia ou continua com o que já usa**.
- **Cliente, não QA**: avalia se vale a pena para a vida dela, não se o software tem defeitos.
- **Realismo**: pouca paciência, leitura rápida, respostas imperfeitas.

Estruturada em estágios, dentro da mesma sessão do agente (guarde o ID de cada um para continuar depois via SendMessage):

1. **Descoberta (primeiros segundos)**: viu o produto/landing. O que acha que é? Para quem? Por que alguém usaria? Interessou (1-5)? Clicaria ou ignoraria?
2. **Experiência** (com app rodando, usa pelo navegador; sem app, avalia o conceito e mockups): tentou resolver **a própria dor**. Chegou ao momento de valor? Em quanto tempo? O que comparou com sua alternativa atual? O que faltou?
3. **Preço e compromisso**: vê o preço previsto. Responde o que acha **barato demais, bom, caro, caro demais** (faixa de preço) e sobe a **escada de compromisso**, parando no primeiro degrau que **não** toparia:
   1. Dar o e-mail · 2. Criar conta · 3. Usar de novo amanhã · 4. Informar o cartão · 5. Pagar o plano · 6. Indicar a alguém de quem gosta, arriscando a reputação.
4. **Uma semana depois**: voltou? O que o faria voltar? O que o faria largar? Trocaria o que usa hoje?

Modelo de instrução do subagente (preencha os campos):

```
Você é [NOME], [idade], [profissão]. [Cartão completo: contexto de vida,
renda, dor, alternativas que usa hoje, letramento digital, dispositivo,
estilo de decisão, postura inicial, voz.]

Você está avaliando, como uma pessoa real, um produto novo: [estímulo:
URL do app / descrição + landing + mockups].

Como se comportar:
- Aja SOMENTE como [NOME] agiria. Use o jeito de falar dele(a).
- A inércia é o padrão: se o produto não te convencer, você ignora, adia
  ou segue com o que já usa. Não seja gentil só por educação.
- Você NÃO é testador de software. Avalia se isso vale a pena para a sua
  vida. Se algo quebrar, só importa se mudar sua opinião ou confiança.
- Não leia código nem arquivos do projeto. Use apenas dados de teste e
  nunca faça pagamento real.
- Não ofereça elogios genéricos. Seja específico(a) e coerente com a sua
  situação e orçamento.

Responda nesta estrutura, em primeira pessoa e na sua voz:
1. DESCOBERTA: o que achei que era, para quem, interesse (1-5), e por quê.
2. EXPERIÊNCIA: o que tentei fazer, se cheguei ao momento de valor, o
   que comparei com o que uso hoje, o que faltou.
3. PREÇO: barato demais / bom / caro / caro demais (em R$) e em qual
   degrau da escada de compromisso eu paro, e por quê.
4. UMA SEMANA DEPOIS: voltei? o que me faria voltar ou largar?
5. OBJEÇÕES e DESEJOS: as 3 maiores objeções e o que me faria dizer sim.
6. FRASE: uma frase, do jeito que eu falaria para um amigo.
```

## Fase 4: Rodada social (inspirada na simulação de rede do MiroFish)

Opinião real se forma em conversa. Depois da rodada individual (que ficou **isolada** para não haver ancoragem):

1. **Monte grupos** de 3 a 5 personas, **mistos** (alvo, adjacente, cético) e alguns **homogêneos** (para ver câmara de eco). Grupos ficam isolados entre si.
2. **Evento-semente**: simule como o produto chega às pessoas: uma recomendação de amigo, um anúncio, um post. Cada persona reage conforme postura e influência.
3. **Rodadas** (padrão 2): para cada grupo, passe por mensagem (SendMessage) o que os outros disseram, em ordem de **influência social**, e colete a resposta de cada persona: mudou de ideia? o que diria a quem perguntasse "vale a pena?"
4. **Observe**: quem **muda de opinião** e por causa de quê, quais objeções se **propagam** e quais **desaparecem**, quem vira defensor ou detrator, e o que as pessoas diriam **espontaneamente** do produto (o texto de boca a boca real do produto).

**Modo econômico**: se o orçamento for curto, um único agente escreve a conversa de cada grupo a partir dos cartões, e o relatório avisa que é menos independente.

## Fase 5: Entrevistas de aprofundamento

Escolha as personas mais **divergentes** ou **surpreendentes** (entusiasta onde se esperava indiferença, cético que quase converteu, desistência no degrau 2) e **continue a sessão delas** com SendMessage para perguntar o "porquê":

- O que exatamente te fez parar?
- O que teria que ser verdade para você pagar?
- O que você usaria no lugar, e por que é melhor para você?
- O que você falaria dessa decisão para um amigo?

## Fase 6 (opcional): E se...? (variantes)

Quando o usuário quiser comparar caminhos (outro preço, outro posicionamento, outra feature, concorrente novo), rode **variantes** com **agentes novos** usando os mesmos cartões (para não carregar a memória da primeira exposição). Compare o **efeito relativo** entre variantes e entre segmentos. Rotule como **direcional**, nunca como previsão de conversão.

## Fase 7: Análise e relatório

Você consolida. Não repasse respostas cruas.

1. **Cruze por segmento** o funil: entendeu → interessou → chegou ao valor → aceitou preço → degraus de compromisso → retorno → indicaria.
2. **Agrupe objeções e desejos** e conte **quantas personas independentes** os citaram. Vários segmentos citando o mesmo ponto = alta confiança; um só = hipótese.
3. **Compare a mensagem recebida com a pretendida**: o que as pessoas acham que o produto é versus o que o dono quer comunicar.
4. **Identifique o segmento de entrada** (*beachhead*): quem entende, valoriza, paga e indica.
5. **Aplique os critérios de decisão** definidos na Fase 1 e dê a recomendação.
6. **Cheque o viés**: proporção de reações positivas, concordância suspeita entre agentes, o que pode ser artefato da simulação.
7. **Use citações** curtas das personas (fictícias) para ilustrar, com ID e segmento.

### Formato do relatório

Omita seções vazias. Escreva para quem decide, em português do Brasil.

```
## Recomendação
LANÇAR · AJUSTAR ANTES · SEGURAR/REPOSICIONAR  (uma delas, com o nível de
confiança: baixa/média/alta) e a justificativa em 3 a 5 linhas.

## Como a simulação foi feita e limites
(Estímulo usado, nº de personas, fases rodadas, evidência de ancoragem
(🟢🟡🔴) e o aviso de que são agentes simulados. Resultado do check de viés.)

## Critérios de decisão (definidos antes)
| Critério | Resultado | Atendido? |

## Elenco
| ID | Persona | Segmento | Dor (1-5) | Postura inicial | Parou no degrau | Voltaria? |

## Funil por segmento
(Entendeu → interessou → valor → preço → compromisso → retorno → indicação,
comparando alvo, adjacentes, fora do alvo e antipersonas)

## Proposta de valor: mensagem pretendida x recebida
(O que entenderam, o que confundiram, e a frase que as pessoas usariam)

## Quem é o público de entrada
(Segmento prioritário e por quê; quem **não** é público)

## Preço
(Faixa percebida de barato a caro, onde a objeção aparece, o que sustenta
o preço, e comparações entre variantes se houve. Tudo direcional.)

## Objeções e o que fazer
| Objeção | Quantas personas / segmentos | Gravidade | Como endereçar |

## O que melhorar antes do lançamento
(Priorizado por impacto na decisão: produto, posicionamento, onboarding,
preço, prova social. Cada item ligado a uma evidência da simulação.)

## Dinâmica social e boca a boca
(O que se espalha, quem influencia, quem vira defensor/detrator, a frase
espontânea de recomendação, riscos de câmara de eco)

## O que funcionou
(Pontos elogiados por vários perfis, que valem proteger)

## Hipóteses para validar com pessoas reais
(5 a 8 perguntas objetivas, o perfil de quem entrevistar, e um teste barato
como landing page com lista de espera ou pré-venda, com o sinal que
confirmaria ou derrubaria cada hipótese)

## Próximos passos
(3 a 5 itens)
```

## Regras

- **O projeto é somente leitura**: não crie, edite nem apague arquivos do projeto. Entregue tudo no chat. Se o usuário quiser salvar o relatório, só grave onde ele indicar.
- **Confirme antes de gastar**: peça aprovação do elenco, das fases e do custo antes de lançar subagentes.
- **Isolamento**: na rodada individual, nenhum agente vê outro. Só na rodada social há troca, e em grupos isolados.
- **Nunca** apresente números da simulação como previsão de conversão, receita ou mercado. Use sempre comparações relativas e linguagem de hipótese.
- **Nunca** invente evidência: cada afirmação sobre o público indica se vem de dado real, pesquisa pública ou inferência do modelo.
- **Privacidade**: material com dados de pessoas reais (reviews, tickets, entrevistas) é resumido e anonimizado. Não copie nomes, e-mails ou telefones para os cartões ou para o relatório.
- **Ambiente seguro**: sem produção com dados reais, pagamento real ou mensagens reais.
- Se um subagente falhar ou não conseguir acessar o app, reporte a falha e siga com as demais personas. Não preencha lacunas com suposição.
- Se o resultado concordar demais com o que o dono esperava, **desconfie** e diga isso.
- Prefira poucos insights bem fundamentados, com quantas personas sustentam cada um, a muitas impressões soltas.
