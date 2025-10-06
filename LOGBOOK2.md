# CVE-2023-3493

## Identificação
**Descrição:** Improper Neutralization of Formula Elements in a CSV File in GitHub repository fossbilling/fossbilling prior to 0.5.3.  
É uma falha na neutralização de fórmulas em campos de dados de utilizador exportados para arquivos CSV/planilhas.  
Ocorre em qualquer sistema que permita a exportação de dados controlados pelo utilizador para CSV.

## Catalogação
- **Publicado em:** 2023-06-30  
- **Gravidade:** 7.7 (Thunt.dev) e 8.0 (NVD)  
- **Reportado via:** plataforma huntr.dev como vulnerabilidade de CSV Injection  
- **Recompensa:** não ocorreu recompensa pública por programas de bug bounty  

## Exploit
- **Tipo de exploit:** CSV Injection  
- **Módulo no Metasploit:** ainda não foi publicado oficialmente  
- **Automatização:** possível ao inserir *payloads* em campos exportados  

### Exemplos de Ataques
- Cliente regista-se e edita perfil, alterando campo Empresa para *payload* de fórmula.  
- Administrador exporta clientes para CSV contendo célula maliciosa.  
- Ao abrir o CSV em Excel, a fórmula é avaliada e o código é executado.  

**Potencial:** execução remota de comandos via fórmula e exfiltração de folhas.  

---

## Correção / Contramedidas

| Tipo | Medida | Descrição curta |
|------|---------|----------------|
| **Patch** | Atualizar para FOSSBilling ≥ 0.5.3 | Aplica `EscapeFormula` no *writer* CSV |
| **Workaround** | Prefixar campos | Usar espaço antes de fórmulas iniciadas por `=`, `+`, `-`, `@` |
| **Mitigação** | Evitar abrir CSVs não sanitizados | Especialmente em aplicações de folha de cálculo |
| **Verificação pós-correção** | Testar payloads | Confirmar que a execução foi bloqueada e a versão corrigida |

---

📄 **Resumo:**  
Esta vulnerabilidade permitia a injeção de fórmulas maliciosas em ficheiros CSV exportados, levando à execução de comandos quando abertos num programa como o Excel.  
Foi corrigida na versão **0.5.3** do FOSSBilling, com a adição da função de *escaping* de fórmulas durante a exportação de dados.
