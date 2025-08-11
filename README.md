# Tutorial: Como Configurar uma Área Inquebrável e Sem PvP no Servidor AnarquiaMC

Olá, Heygasttk! Este tutorial vai te guiar passo a passo para configurar uma área de spawn **inquebrável** (onde ninguém pode quebrar ou colocar blocos) e **sem PvP** no mundo `world_anarquia` do nosso servidor **Paper 1.21.8**, hospedado no Pterodactyl com o subdomínio `anarquiamc.redegamer.com.br`. O servidor suporta jogadores Java (porta `26002`) e Bedrock (porta `26008`) usando os plugins **Geyser** e **Floodgate**, além de **ViaVersion** e **ViaBackwards** para compatibilidade com versões mais antigas do Minecraft Java. Vamos usar o plugin **WorldGuard** para criar a área protegida. O tutorial também inclui dicas para evitar problemas, como lag e desconexões, que apareceram nos logs do servidor.

---

## Pré-requisitos
- **Acesso ao Servidor**: Você é um operador (OP) no servidor, conforme registrado nos logs (`Made Heygasttk a server operator`). Isso te dá permissão para usar todos os comandos necessários.
- **Acesso ao Painel Pterodactyl**: Você precisará do login do painel Pterodactyl para instalar plugins. Se não tiver acesso, peça ao administrador do servidor.
- **Minecraft Java**: Use o Minecraft Java 1.21.8 para configurar a área, conectando em `anarquiamc.redegamer.com.br:26002`.
- **Minecraft Bedrock**: Para testar a área com Bedrock, conecte em `anarquiamc.redegamer.com.br:26008`. Seu nome aparecerá como `.Heygasttk` devido ao plugin Floodgate.

---

## Passo 1: Instale o WorldGuard e WorldEdit
Para criar a área inquebrável e sem PvP, usaremos o **WorldGuard**, que precisa do **WorldEdit** para selecionar regiões facilmente. Esses plugins são compatíveis com o Paper 1.21.8 e funcionam com jogadores Bedrock via Geyser/Floodgate.

1. **Baixe os Plugins**:
   - **WorldGuard**: Acesse [enginehub.org](https://worldguard.enginehub.org) ou [spigotmc.org](https://www.spigotmc.org/resources/worldguard.1042/) e baixe a versão mais recente para Minecraft 1.21.8 (ex.: `WorldGuard-7.1.x.jar`).
   - **WorldEdit**: Acesse [enginehub.org](https://worldedit.enginehub.org) ou [spigotmc.org](https://www.spigotmc.org/resources/worldedit.1391/) e baixe a versão para 1.21.8 (ex.: `WorldEdit-7.3.x.jar`).
2. **Faça Upload no Pterodactyl**:
   - Acesse o painel Pterodactyl do servidor.
   - Vá para a aba **Files** > pasta `plugins`.
   - Clique em **Upload** e selecione os arquivos `WorldGuard.jar` e `WorldEdit.jar`.
3. **Reinicie o Servidor**:
   - Na aba **Manage**, clique em **Restart**.
   - Verifique na aba **Console** ou no arquivo `logs/latest.log` se os plugins carregaram corretamente. Você verá mensagens como:
     ```
     [INFO]: [WorldEdit] Enabling WorldEdit v7.3.x
     [INFO]: [WorldGuard] Enabling WorldGuard v7.1.x
     ```
4. **Resolva Problemas**:
   - Se houver erros nos logs, peça ajuda ao administrador ou envie os logs para o suporte do GeyserMC ([discord.gg/geysermc](https://discord.gg/geysermc)).

---

## Passo 2: Defina o Ponto de Spawn
A área protegida será no spawn do mundo `world_anarquia`. Vamos definir o ponto de spawn onde os jogadores aparecerão.

1. **Conecte ao Servidor**:
   - Abra o Minecraft Java 1.21.8.
   - Adicione o servidor:
     - **Endereço**: `anarquiamc.redegamer.com.br:26002`
     - **Nome**: AnarquiaMC (ou qualquer nome).
   - Conecte como `Heygasttk`.
2. **Escolha o Local do Spawn**:
   - Vá para o centro da área que você quer proteger (ex.: coordenadas `0, 64, 0` no mundo `world_anarquia`).
   - Use o comando para definir o spawn:
     ```
     /setworldspawn 0 64 0
     ```
   - Isso faz com que novos jogadores apareçam nesse ponto.
3. **Teste o Spawn**:
   - Desconecte e reconecte para confirmar que você aparece no local correto.
   - Teste também no Bedrock (`anarquiamc.redegamer.com.br:26008`) como `.Heygasttk`.

---

## Passo 3: Crie a Área Inquebrável e Sem PvP
Usaremos o **WorldGuard** para criar uma região protegida no spawn, onde ninguém (exceto operadores) pode quebrar/colocar blocos ou fazer PvP.

1. **Selecione a Área**:
   - **Com a Varinha (WorldEdit)**:
     - Pegue um graveto (ou use `/wand` para obter a varinha do WorldEdit).
     - Clique com o **botão esquerdo** no primeiro canto da área (ex.: `-25, 60, -25`).
     - Clique com o **botão direito** no segundo canto oposto (ex.: `25, 80, 25`).
     - Isso seleciona um cubo de 50x50 blocos, com altura de 20 blocos (ajuste conforme necessário).
   - **Com Comandos (sem varinha)**:
     - Defina os dois cantos opostos:
       ```
       /pos1 -25 60 -25
       /pos2 25 80 25
       ```
2. **Crie a Região**:
   - Nomeie a região como `spawn`:
     ```
     /rg define spawn
     ```
3. **Torne a Área Inquebrável**:
   - Proíba a quebra e colocação de blocos:
     ```
     /rg flag spawn build deny
     ```
4. **Desative o PvP**:
   - Proíba o PvP na região:
     ```
     /rg flag spawn pvp deny
     ```
5. **Flags Adicionais (Recomendado)**:
   - Impedir danos de monstros (ex.: zumbis, esqueletos):
     ```
     /rg flag spawn mob-damage deny
     ```
   - Impedir explosões (ex.: creepers, TNT):
     ```
     /rg flag spawn explosion deny
     ```
   - Proteger contra outros danos (ex.: lava, queda):
     ```
     /rg flag spawn damage deny
     ```
6. **Permitir Construção Apenas para Operadores (Opcional)**:
   - Se quiser que apenas operadores (como você, `Heygasttk`) possam construir:
     ```
     /rg flag spawn build -g nonmembers deny
     /rg addmember spawn g:operators
     ```
7. **Salve as Configurações**:
   - Execute:
     ```
     /rg save
     ```
   - Isso salva a região em `plugins/WorldGuard/worlds/world_anarquia/regions.yml`.

---

## Passo 4: Teste a Região Protegida
1. **Teste no Java**:
   - Conecte em `anarquiamc.redegamer.com.br:26002`.
   - Vá para a área do spawn (ex.: `0, 64, 0`).
   - Tente quebrar ou colocar blocos (não deve funcionar, exceto para você, que é OP).
   - Tente atacar outro jogador (PvP deve estar desativado).
2. **Teste no Bedrock**:
   - Conecte em `anarquiamc.redegamer.com.br:26008` como `.Heygasttk`.
   - Repita os testes de quebra, colocação de blocos e PvP.
   - Confirme que a área aparece corretamente (o Geyser/Floodgate garante compatibilidade).
3. **Verifique Problemas**:
   - Se algo não funcionar (ex.: jogadores conseguem quebrar blocos), use:
     ```
     /rg info spawn
     ```
     - Isso mostra as flags da região. Ajuste com `/rg flag` se necessário.
   - Cheque os logs (**Console** ou `logs/latest.log`) para erros relacionados ao WorldGuard.

---

## Passo 5: Dicas para Manter o Servidor Estável
Os logs do servidor mostraram problemas de desempenho e desconexões, especialmente para jogadores Bedrock. Aqui estão algumas dicas para evitar lag e garantir que a área protegida funcione bem:

1. **Corrigir Plugin Corrompido**:
   - Os logs mostraram um erro com `lullaby-graves-v2.1.4.jar`:
     ```
     [20:04:23 ERROR]: Directory 'plugins/.paper-remapped/lullaby-graves-v2.1.4.jar' does not contain a paper-plugin.yml or plugin.yml!
     ```
   - **Ação**:
     - No Pterodactyl, vá para **Files** > `plugins/.paper-remapped`.
     - Delete o arquivo `lullaby-graves-v2.1.4.jar`.
     - Se quiser usar o plugin **Graves**, baixe a versão correta de [spigotmc.org](https://www.spigotmc.org/resources/graves.82738/) e faça upload para `plugins` (não `.paper-remapped`).
     - Reinicie o servidor.
2. **Reduzir Lag**:
   - Os logs mostram avisos de sobrecarga:
     ```
     [20:47:32 WARN]: Can't keep up! Is the server overloaded? Running 9802ms or 196 ticks behind
     ```
   - **Ação**:
     - Abra **Files** > `server.properties` e ajuste:
       ```properties
       view-distance=6
       simulation-distance=6
       ```
     - Abra **Files** > `paper.yml` e ajuste:
       ```yaml
       chunk-system:
         gen-parallelism: default
         max-chunk-sends-per-tick: 4
       ```
     - Salve e reinicie.
     - Use o comando `/spark` no jogo para verificar o que está causando lag (veja resultados em [spark.lucko.me](https://spark.lucko.me/)).
     - Se o lag persistir, peça ao administrador para contatar a RedeGamer e solicitar mais RAM/CPU (recomendo 4-6 GB para 10-20 jogadores).
3. **Evitar Desconexões no Bedrock**:
   - Os logs mostram timeouts de jogadores Bedrock:
     ```
     [20:44:36 INFO]: Heygasttk has disconnected from the Java server because of Bedrock client timed out
     ```
   - **Ação**:
     - Confirme que a porta UDP `26008` está liberada (peça ao administrador para testar com [portchecker.co](https://portchecker.co)).
     - Atualize os plugins **Geyser** e **Floodgate**:
       - Baixe as versões mais recentes de [geysermc.org](https://geysermc.org).
       - Substitua `Geyser-Spigot.jar` e `Floodgate-Spigot.jar` em `plugins` e reinicie.
     - Em `paper.yml`, ajuste para reduzir falsos positivos de movimento rápido:
       ```yaml
       player-movement:
         speed-check-margin: 0.1
       ```
4. **Segurança**:
   - O servidor está em modo offline (`online-mode=false`), necessário para o Floodgate, mas isso permite logins com nomes falsos no Java.
   - **Ação**:
     - Considere instalar **LuckPerms** ([spigotmc.org](https://www.spigotmc.org/resources/luckperms.28140/)) para gerenciar permissões:
       - Faça upload de `LuckPerms.jar` para `plugins`.
       - Use `/lp editor` para criar ranks (ex.: Jogador, Admin).
     - Se não precisar de OP, remova suas permissões de operador para maior segurança:
       ```
       /deop Heygasttk
       /deop .Heygasttk
       ```
5. **Backup Regular**:
   - Após configurar a área, crie um backup no Pterodactyl (**Backup** > **Create Backup**) para proteger o mundo `world_anarquia`.

---

## Passo 6: O Que Fazer se Algo Der Errado
- **Região Não Funciona**:
  - Use `/rg info spawn` para verificar as flags.
  - Ajuste com `/rg flag spawn <flag> <valor>` (ex.: `/rg flag spawn build deny`).
- **Lag ou Desconexões**:
  - Execute `/spark` e compartilhe os resultados com o administrador.
  - Verifique os logs (**Console** ou `logs/latest.log`) e envie erros ao administrador.
- **Problemas com Bedrock**:
  - Se a área não funcionar para jogadores Bedrock (ex.: você como `.Heygasttk`), use `/geyser dump` e envie os resultados ao suporte do GeyserMC ([discord.gg/geysermc](https://discord.gg/geysermc)).
- **Erro nos Plugins**:
  - Se o WorldGuard ou WorldEdit não carregar, cheque os logs e peça ajuda ao administrador.
- **Mundo Antigo (AnarquiaMC)**:
  - Se quiser voltar ao mundo `AnarquiaMC`, peça ao administrador para restaurar o backup ou usar o plugin **Multiverse-Core** para carregar múltiplos mundos:
    - Baixe de [spigotmc.org](https://www.spigotmc.org/resources/multiverse-core.390/).
    - Use `/mv import world_anarquia_old normal` para carregar o mundo antigo.

---

## Resumo dos Comandos
- Definir spawn: `/setworldspawn 0 64 0`
- Selecionar área: `/pos1 -25 60 -25` e `/pos2 25 80 25` (ou use a varinha do WorldEdit)
- Criar região: `/rg define spawn`
- Flags:
  - Inquebrável: `/rg flag spawn build deny`
  - Sem PvP: `/rg flag spawn pvp deny`
  - Opcional: `/rg flag spawn mob-damage deny`, `/rg flag spawn explosion deny`
  - Apenas OPs constroem: `/rg flag spawn build -g nonmembers deny`, `/rg addmember spawn g:operators`
- Salvar: `/rg save`

---

## Suporte
Se precisar de ajuda, entre em contato comigo ou com o administrador do servidor. Para problemas com Bedrock, o Discord do GeyserMC ([discord.gg/geysermc](https://discord.gg/geysermc)) é uma boa opção. Se o servidor continuar com lag ou desconexões, peça ao administrador para verificar os recursos (RAM/CPU) com a RedeGamer.

Boa sorte configurando o spawn do AnarquiaMC! 🎮
