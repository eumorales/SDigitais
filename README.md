# TrabalhoFinal
# 🏠 Projeto: Sistema de Alarme Residencial  
**Integrantes:**  Gabriel Saccol e Gilberto Morales  

---

## 🎯 Objetivo do Circuito
O objetivo deste projeto é desenvolver, em **VHDL utilizando o Vivado**, um **sistema de alarme residencial digital** capaz de representar três estados principais de operação:  
1. **Desativado** – o sistema está inativo, sem monitoramento;  
2. **Ativado** – o sistema está em vigilância, pronto para detectar invasões;  
3. **Disparado** – o alarme é acionado após a detecção de uma invasão.  

O projeto busca aplicar conceitos de **Máquinas de Estados Finitos (FSM)**, **portas lógicas**, **flip-flops** e **simulação digital**, simulando o comportamento de um sistema de segurança real.

---

## 🧠 Diagrama de Estados (FSM)
A FSM (Finite State Machine) do sistema possui **três estados** principais e **transições controladas por sinais de entrada**:

       +-------------------+
       |                   |
       |   [S1] DESATIVADO |
       |                   |
       +---------+---------+
                 |
         (botão ligar)
                 v
       +-------------------+
       |                   |
       |   [S2] ATIVADO    |
       |                   |
       +---------+---------+
                 |
         (sensor = 1)
                 v
       +-------------------+
       |                   |
       |   [S3] DISPARADO  |
       |                   |
       +---------+---------+
                 |
         (botão reset)
                 v
       +-------------------+
       |   DESATIVADO      |
       +-------------------+


---

## ⚙️ Explicação do Funcionamento Passo a Passo

1. **Estado Desativado (S1)**  
   - O sistema inicia neste estado.  
   - O alarme está inativo.  
   - Ao pressionar o **botão de ativar**, o sistema muda para o estado **Ativado**.  

2. **Estado Ativado (S2)**  
   - O alarme está monitorando o ambiente.  
   - Caso o **sensor** detecte uma presença (sinal = ‘1’), o estado muda para **Disparado**.  
   - Caso contrário, permanece **Ativado**.  

3. **Estado Disparado (S3)**  
   - O alarme é acionado (saída = ‘1’).  
   - Para desligar o alarme, é necessário pressionar o **botão de reset**, retornando ao estado **Desativado**.  

---

## 💻 Estrutura do Repositório

---

## 🧩 Funcionamento Interno (Descrição Técnica)
- **Entradas:**  
  - `clk`: sinal de clock.  
  - `reset`: retorna o sistema ao estado desativado.  
  - `ativar`: ativa o sistema.  
  - `sensor`: sinal de detecção.  

- **Saídas:**  
  - `alarme`: indica se o alarme está disparado (1 = ligado, 0 = desligado).  
  - `estado`: mostra o estado atual (00 = desativado, 01 = ativado, 10 = disparado).  

- **Tecnologia usada:** VHDL (linguagem de descrição de hardware)  
- **Software:** Xilinx Vivado 2015.1  

---

## Código VHDL 

       -- alarme.vhd
       library ieee;
       use ieee.std_logic_1164.all;

       entity alarme is
    port (
        clk    : in  std_logic;
        reset  : in  std_logic;  -- reset assíncrono ativo em '1' -> volta para DESATIVADO
        ativar : in  std_logic;  -- botão para ativar vigilância
        sensor : in  std_logic;  -- detector (1 = presença detectada)
        alarme : out std_logic;  -- 1 = alarme disparado
        estado : out std_logic_vector(1 downto 0) -- 00=desativado, 01=ativado, 10=disparado
    );
       end entity;

       architecture rtl of alarme is

    -- codificação dos estados
    constant S_DES : std_logic_vector(1 downto 0) := "00";
    constant S_ATV : std_logic_vector(1 downto 0) := "01";
    constant S_DIS : std_logic_vector(1 downto 0) := "10";

    signal cur_state, next_state : std_logic_vector(1 downto 0);

       begin

    ------------------------------------------------------------------------
    -- Processo de registro (flip-flops) com reset assíncrono
    ------------------------------------------------------------------------
    proc_reg : process(clk, reset)
    begin
        if reset = '1' then
            cur_state <= S_DES;

        elsif rising_edge(clk) then
            cur_state <= next_state;
        end if;
    end process;

    ------------------------------------------------------------------------
    -- Lógica de próxima condição (combinacional)
    ------------------------------------------------------------------------
    proc_next : process(cur_state, ativar, sensor)
    begin
        case cur_state is

            when S_DES =>
                if ativar = '1' then
                    next_state <= S_ATV;
                else
                    next_state <= S_DES;
                end if;

            when S_ATV =>
                if sensor = '1' then
                    next_state <= S_DIS;
                else
                    next_state <= S_ATV;
                end if;

            when S_DIS =>
                -- permanece em disparado até o reset assíncrono
                next_state <= S_DIS;

            when others =>
                next_state <= S_DES;

        end case;
    end process;

    ------------------------------------------------------------------------
    -- Saídas
    ------------------------------------------------------------------------
    alarme <= '1' when cur_state = S_DIS else '0';
    estado <= cur_state;

       end architecture;



## 🧾 Conclusão

Durante o desenvolvimento deste projeto, foi possível compreender de forma prática:
- A implementação de **máquinas de estados** em VHDL;  
- O uso de **simulações digitais** para validar o comportamento do sistema;  
- A importância da **organização do projeto no Vivado**;  
- A relação entre **entradas, estados e saídas** em sistemas digitais.  

### 🧠 Aprendizados
- Controle de estados em VHDL e uso de processos síncronos.  
- Depuração de sinais usando **waveform simulation**.  
- Estruturação de um projeto digital completo no Vivado.

### ⚠️ Dificuldades
- Ajuste correto do **clock** na simulação.  
- Interpretação das formas de onda.  
- Entendimento inicial da transição de estados com base nas entradas.

### Obrigado Professor!
---

