# Sorteio de Rodadas

Sistema web para gerenciamento de sorteios e escolhas de rodadas entre participantes, com sincronização em tempo real e modo manual de escolhas.

## 🌐 Ambientes

- **Produção**: https://sorteio-escolha.vercel.app
- **Desenvolvimento**: https://sorteio-escolha-ddg27ghz7-andersonbernardos-projects.vercel.app

## ✨ Funcionalidades

### Gestão de Sorteios
- **Lobby de Sorteios**: Visualização e seleção de sorteios salvos
- **Criação de Sorteios**: Criação de novos sorteios com nomes personalizados
- **Sincronização em Tempo Real**: Dados sincronizados automaticamente via API (npoint.io)
- **Múltiplos Sorteios**: Gerenciamento de vários sorteios simultaneamente

### Configuração de Participantes
- **Número de Participantes**: Configurável (mínimo 2)
- **Nomes e E-mails**: Suporte para cadastro de nomes e e-mails opcionais
- **Randomização de Ordem**: Embaralhamento automático dos participantes
- **Identificação de Usuários**: Sistema que associa dispositivos aos jogadores

### Modos de Jogo

#### Modo Automático
- Sorteio automático de números (1-100) para cada participante
- Determinação automática de vencedores
- Critério de vitória configurável (maior ou menor número)

#### Modo Manual
- Jogadores escolhem números na ordem determinada por sorteio
- Sistema de turnos com reivindicação (claim)
- Controle de quem está jogando no momento
- Exibição do número sorteado para cada jogador (ordem de escolha)

### Critérios de Vitória
- **Critério Fixo**: Sempre vence o maior (ou menor) número
- **Critério Randomizado**: A cada rodada é sorteado se vence MAIOR ou MENOR

### 🍫 Nutella - Bad Luck Protection

Sistema de balanceamento que ajusta as chances dos jogadores baseado em desempenho anterior.

#### Como Funciona
- **1º lugar**: sofre nerf de **20 pontos**
- **2º lugar**: sofre nerf de **10 pontos**
- **Penúltimo** (apenas com 4+ jogadores): ganha buff de **10 pontos**
- **Último** (apenas com 4+ jogadores): ganha buff de **20 pontos**

#### Regras Especiais
- **Com 3 jogadores ou menos**: apenas 1º e 2º sofrem nerf (sem buffs)
- **Com 4+ jogadores**: sistema completo de nerfs e buffs

#### Aplicação por Critério
- **Critério MAX (MAIOR vence)**:
  - Nerf: diminui o número sorteado
  - Buff: aumenta o número sorteado

- **Critério MIN (MENOR vence)**:
  - Nerf: aumenta o número sorteado
  - Buff: diminui o número sorteado

#### Exibição Visual
- Valores acumulados aparecem no **cabeçalho** da tabela (linha PESSOA 1, PESSOA 2, etc.)
- **Verde** para buffs positivos
- **Vermelho** para nerfs negativos
- Mostra apenas o valor absoluto: `PESSOA 1 (20)` sem sinal + ou -
- Os valores **acumulam** durante todo o sorteio

#### Exemplo
```
SILAS (40)     MISTER (10)    CHINA (0)     JEGA (20)
[verde]        [verde]                      [vermelho]
```

### Recursos Adicionais
- **Exportação/Importação**: Backup e restauração de dados em formato JSON
- **Proteção por Senha**: Operações críticas protegidas (senha padrão: `PES123`)
- **Interface Responsiva**: Design adaptável para diferentes tamanhos de tela
- **Tema Escuro**: Interface com esquema de cores escuro
- **Indicadores Visuais**: Marcação de 1º, 2º e 3º lugares com cores

## 🚀 Tecnologias

- **HTML5**: Estrutura da página
- **CSS3**: Estilização e layout responsivo
  - CSS Variables para temas
  - Flexbox e Grid para layout
- **JavaScript (Vanilla)**: Lógica da aplicação
  - Crypto API para geração de números aleatórios seguros
  - Fetch API para comunicação com backend
  - LocalStorage para identificação de usuários
- **Vercel**: Deploy e hospedagem
- **npoint.io**: Armazenamento JSON em nuvem

## 📦 Deploy

### Desenvolvimento
```bash
# Fazer alterações na branch nutella-mister
git checkout nutella-mister
# ... suas alterações ...
git add .
git commit -m "feat: nova funcionalidade"
git push
vercel  # deploy automático
```

### Produção
```bash
# Merge para main
git checkout main
git merge nutella-mister
git push
vercel --prod
```

## 🔧 Comandos Úteis

```bash
# Listar deploys
vercel ls

# Ver logs
vercel logs

# Inspecionar deploy
vercel inspect [url]

# Redeployar
vercel redeploy [url]
```

## 🏗️ Estrutura de Dados

```javascript
{
  config: {
    idnpoint: "id-do-sorteio",
    players: 4,
    randomize_win_criteria: false,
    manual_mode: false,
    nutella_mode: true,
    updatedAt: "2025-01-17T..."
  },
  columns: ["P1", "P2", "P3", "P4"],
  names: {
    "P1": {
      name: "JOGADOR 1",
      email: "jogador1@example.com",
      knownIds: ["user-uuid"]
    }
  },
  nutella_buffs: {
    "P1": -20,
    "P2": 10,
    "P3": 0,
    "P4": 20
  },
  rounds: [
    {
      id: "R1",
      criterion: "MAX",
      values: { "P1": 75, "P2": 42, "P3": 88, "P4": 63 },
      order: ["P3", "P1", "P4", "P2"],
      nro_sorteado: { ... },
      claimedBy: { ... }
    }
  ]
}
```

## 🔐 Segurança

### Senha Padrão
As seguintes operações requerem senha (`PES123`):
- Criar novo sorteio
- Gerar nova rodada

### Identificação de Jogadores
- Cada dispositivo recebe um ID único (UUID)
- O sistema associa o ID ao jogador na primeira escolha
- Previne que um jogador escolha por outro

## 📱 Compatibilidade

### Navegadores Suportados
- Chrome/Edge (v90+)
- Firefox (v88+)
- Safari (v14+)

### Requisitos
- JavaScript habilitado
- Conexão com internet (para sincronização)
- Suporte a localStorage
- Suporte a Crypto API

## 🤝 Contribuindo

1. Faça fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/MinhaFeature`)
3. Commit suas mudanças (`git commit -m 'feat: Adiciona MinhaFeature'`)
4. Push para a branch (`git push origin feature/MinhaFeature`)
5. Abra um Pull Request

## 📝 Licença

Este é um projeto educacional/experimental. Uso livre para fins não comerciais.

## 🔄 Changelog

### v2.0.0 - Nutella Update
- ✨ Adiciona sistema Nutella (Bad Luck Protection)
- ✨ Buffs/nerfs acumulativos baseados em colocação
- ✨ Indicadores visuais no cabeçalho da tabela
- ✨ Regras especiais para 3 jogadores ou menos
- 🐛 Corrige configurações CORS para todas as chamadas fetch
- 📦 Deploy automático com Vercel
- 🌐 Ambiente de desenvolvimento separado

### v1.0.0
- ✨ Sistema básico de sorteio
- ✨ Modo manual e automático
- ✨ Sincronização em tempo real
- ✨ Gestão de múltiplos sorteios

---

**Versão**: 2.0.0 (Nutella Update)
**Última atualização**: Janeiro 2025
