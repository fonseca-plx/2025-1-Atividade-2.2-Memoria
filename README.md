# S.O. 2025.1 - Atividade 2.02 - Gestão de memória

## Informações gerais

- **Objetivo do repositório**: Repositório para atividade avaliativa dos alunos
- **Assunto**: Gestão de memória
- **Público alvo**: alunos da disciplina de SO (Sistemas Operacionais) do curso de TADS (Superior em Tecnologia em Análise e Desenvolvimento de Sistemas) no CNAT-IFRN (Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte - Campus Natal-Central).
- disciplina: **SO** [Sistemas Operacionais](https://github.com/sistemas-operacionais/)
- professor: [Leonardo A. Minora](https://github.com/leonardo-minora)
- Repositótio do aluno: FIXME

## Tarefas do aluno
1. Fork desse repositório e atualizar a linha 10 com o nome e link do github
2. Ler a descrição da atividade
3. Montar a resposta no final deste arqivo, no tópico **Resposta**

---

## 1. Descrição da atividade
### 1.1. Objetivo
Praticar os conceitos de alocação de memória (best-fit), memória virtual e desfragmentação em um sistema com memória limitada.

---

### 1.2. Contexto
Um computador possui apenas **64 KB de RAM** e um **disco rígido para memória virtual**. O sistema operacional deve gerenciar 5 processos com tamanhos diferentes, cuja soma ultrapassa a capacidade da RAM.

#### 1.2.1. Processos iniciais

| Processo | Tamanho (KB) |
|----------|-------------|
| P1       | 20          |
| P2       | 15          |
| P3       | 25          |
| P4       | 10          |
| P5       | 18          |
| **Total**| **88 KB**   |

- **Memória RAM**: 64 KB (contígua, inicialmente vazia).  
- **Memória Virtual (Disco)**: Espaço ilimitado para paginação.

#### 1.2.2. Alocação Inicial com Best-Fit
Os alunos devem simular a alocação dos processos na RAM usando o algoritmo **best-fit**.  
- A memória RAM será representada como um bloco contíguo (ex: `[0KB - 64KB]`).  
- Devem alocar os processos nos menores espaços livres que atendam ao seu tamanho.  

**Alocação inicial**:  
1. P1 (20 KB) → Ocupa [0-20].  
2. P2 (15 KB) → Ocupa [20-15].  
3. _continuar a partir daqui_

#### 1.2.3. Simular Memória Virtual (Paginação)
- Os processos não alocados na RAM devem ser "paginados" no disco.  
- Criar uma tabela de páginas indicando quais partes estão na RAM e quais estão no disco.  

#### 1.2.4. Desfragmentação da RAM
- Desfragmentar a RAM para liberar espaço contíguo.
- Após desfragmentação (compactação), verificar quais processos podem ser alocado.  

### 1.3. Questões para Reflexão
1. Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?  
2. Como a memória virtual evitou um deadlock?  
3. Qual o impacto da desfragmentação no desempenho do sistema?  

---

## Resposta

## **1. Alocação Inicial com Best-Fit**

### Passo a passo

1. **P1 (20 KB)**  
   - Espaços livres: [0–64] (64 KB livre)  
   - Escolhe [0–20].  
   - RAM: `[P1(20)] [livre(44)]`

2. **P2 (15 KB)**  
   - Espaços livres: [20–64] (44 KB livre)  
   - Escolhe [20–35].  
   - RAM: `[P1(20)] [P2(15)] [livre(29)]`

3. **P3 (25 KB)**  
   - Espaços livres: [35–64] (29 KB livre)  
   - Escolhe [35–60].  
   - RAM: `[P1(20)] [P2(15)] [P3(25)] [livre(4)]`

4. **P4 (10 KB)**  
   - Espaços livres: [60–64] (4 KB livre) → não cabe.  
   - Vai para memória virtual (disco).

5. **P5 (18 KB)**  
   - Espaços livres: [60–64] (4 KB livre) → não cabe.  
   - Vai para memória virtual (disco).

### Resultado inicial

**RAM (64 KB):**
```
[0–20]  P1 (20 KB)
[20–35] P2 (15 KB)
[35–60] P3 (25 KB)
[60–64] Livre (4 KB)
```

**Memória Virtual (Disco):**
```
P4 (10 KB)
P5 (18 KB)
```

---

## **2. Simulação da Memória Virtual (Paginação)**

| Processo | Páginas na RAM   | Páginas no Disco         |
|----------|------------------|--------------------------|
| P1       | 0–19 KB          | —                        |
| P2       | 20–34 KB         | —                        |
| P3       | 35–59 KB         | —                        |
| P4       | —                | 0–9 KB (no disco)        |
| P5       | —                | 0–17 KB (no disco)       |

> Representação simplificada: cada processo é tratado como um único bloco, sem subdivisão real em páginas de tamanho fixo.

---

## **3. Desfragmentação da RAM**

### Antes
```
[ P1 (20 KB) ][ P2 (15 KB) ][ P3 (25 KB) ][ Livre (4 KB) ]
```

### Após desfragmentação
```
[ P1 (20 KB) ][ P2 (15 KB) ][ P3 (25 KB) ][ Livre (4 KB) ]
```

- A memória já estava organizada com todo o espaço livre no final.  
- Espaço livre final = **4 KB** (insuficiente para P4 ou P5).  
- Nenhum novo processo pode ser alocado após a compactação.

---

## **4. Questões para Reflexão**

1. **Best-fit foi mais eficiente que first-fit ou worst-fit neste cenário?**  
   - Não. Como a memória começou vazia e os processos foram carregados em sequência, best-fit teve o mesmo resultado que first-fit.  
   - Worst-fit não seria vantajoso e poderia aumentar a fragmentação interna.

2. **Como a memória virtual evitou um deadlock?**  
   - Sem memória virtual, P4 e P5 ficariam bloqueados indefinidamente aguardando espaço na RAM.  
   - A paginação no disco permitiu que o sistema continuasse executando, armazenando processos não alocados na RAM.

3. **Qual o impacto da desfragmentação no desempenho do sistema?**  
   - Reduz fragmentação externa e pode viabilizar novas alocações.  
   - Neste caso, não trouxe benefício pois a área livre já estava contígua.  
   - Em sistemas reais, desfragmentar consome tempo de CPU e movimenta dados na RAM.
