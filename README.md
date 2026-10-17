# Cadete PMESP — Central de Estudos (PWA) 0.1

Aplicativo web progressivo para Android e Windows 11, em português.

## O que já funciona
- Criar, concluir, filtrar e excluir tarefas com matéria, atividade e data.
- Armazenamento local via `localStorage`; dados continuam disponíveis no mesmo navegador após fechar/reabrir.
- Interface adaptável para celular e desktop.
- Service worker com cache dos arquivos para abrir offline após a primeira visita online.
- Pomodoro com presets de 25, 45, 50 e 5 minutos, pausar/zerar e registro de sessões concluídas.
- Estatísticas de tarefas e sessões; sugestões de revisão em 1, 7 e 30 dias para tarefas concluídas.
- Exportação de tarefas/sessões em Markdown para guardar no vault do Obsidian.
- Backup e restauração JSON.
- Aviso de estado online/offline.

## Limitações conhecidas desta primeira versão
- **Sincronização na nuvem ainda não está ligada.** O aplicativo não transmite dados para servidor. Para sincronizar Android e Windows é necessário configurar um backend autenticado (por exemplo, Firebase Auth + Firestore ou Supabase) e uma estratégia de resolução de conflitos. Não coloque chaves privadas em código público.
- O exportador Markdown baixa um arquivo; não grava diretamente no vault do Obsidian.
- O cronômetro funciona com a página ativa; navegadores podem suspender timers em segundo plano. Notificações exigem permissão e HTTPS; a entrega em segundo plano não é garantida.
- Revisões são lembretes básicos baseados na data de conclusão, não um algoritmo adaptativo.
- Dados locais são específicos de cada navegador/aparelho; limpar dados do navegador pode apagá-los. Faça backups.
- Não é um APK nativo nem um instalador MSI/EXE; é um PWA instalável pelo navegador.

## Testar localmente
O PWA precisa de HTTPS ou `localhost` para o service worker. Com Python instalado:
```bash
python -m http.server 8000
```
Abra `http://localhost:8000` no mesmo computador. Para testar no celular, publique primeiro num host HTTPS.

## Publicar
1. Crie um repositório GitHub e envie o conteúdo desta pasta.
2. No GitHub, abra Settings → Pages, escolha a branch principal e a pasta raiz, e salve.
3. Aguarde o endereço HTTPS do Pages e abra-o no celular e no PC.
4. A URL deve servir `index.html`, `manifest.webmanifest`, `sw.js`, `styles.css`, `app.js` e `icons/`.
5. Se publicar sob um caminho de projeto, mantenha os caminhos relativos (`./`) como estão.

## Instalar
### Android
1. Abra a URL HTTPS no Chrome.
2. Menu ⋮ → “Instalar app” ou “Adicionar à tela inicial”.
3. Confirme e abra pelo ícone.

### Windows 11
1. Abra a URL HTTPS no Microsoft Edge ou Chrome.
2. Use o ícone de instalação na barra de endereços ou menu → Apps → instalar este site como aplicativo.
3. Fixe na barra de tarefas, se quiser.

## Conectar sincronização em nuvem (próxima etapa)
Implementar login e sincronização em um serviço backend. Requisitos recomendados:
- autenticação por usuário;
- coleção/documento por usuário, com regras que impeçam leitura/escrita de dados alheios;
- fila local para mudanças offline e sincronização quando voltar a conexão;
- `updatedAt`/versão por registro e tratamento de conflitos;
- exportação/backup antes de migrações.
Não habilite Firestore com regras públicas de leitura/escrita.

## Estrutura
- `index.html`: interface.
- `styles.css`: estilos responsivos.
- `app.js`: tarefas, timer, estatísticas, exportação e backup.
- `sw.js`: cache offline.
- `manifest.webmanifest`: metadados de instalação.
- `icons/`: ícones PWA.

## Novidades da v0.2
- **Importar checklist** (HTML/Markdown/texto) na aba Hoje, com data inicial e itens por dia.
- **Log automático para Obsidian**: cada Pomodoro e tarefa concluída vira linha em `pasta/AAAA-MM-DD.md`.
  - PC (Edge/Chrome): "Escolher pasta do vault" grava direto no arquivo (pode pedir permissão de novo após reabrir; use "Obsidian ⟳").
  - Android: informe o nome do vault; "Obsidian ⟳" abre o app com `obsidian://new?...&append=true` (um dia por toque). Requer o Obsidian instalado.
- Linhas pendentes ficam guardadas no backup JSON. Cache do service worker atualizado para v2.
