# [Challenge Name]
**Date**: 2025-05-13 
**Category**: Hardware

## Table of Contents
- [Summary](#summary)
- [Reconnaissance & Enumeration](#reconnaissance--enumeration)
- [Exploitation](#exploitation)
- [Takeaways & Lessons Learned](#takeaways--lessons-learned)

---

## Summary
I was given a docker container to connect and interact with using nc. It asks for 16-bit binary for input and returns output as 16-bit as well.

---

## Reconnaissance & Enumeration
I was given some downloadable files to examine, which included many .vhdl files and a png file of the schematic. After reading through it, I was confident that the CTF wanted the user to find a way to gain the flag through a specific input. After looking through the files, I noticed a backdoor.vhdl file.
```bash
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.NUMERIC_STD.ALL;

entity backdoor is
	Port (
		D : in STD_LOGIC_VECTOR(15 downto 0);
		B : out STD_LOGIC
	);
end backdoor;

architecture Behavioral of backdoor is
    constant pattern : STD_LOGIC_VECTOR(15 downto 0) := "1111111111101001";
begin
	process(D)
	begin
        if D = pattern then
            B <= '1';
        else
            B <= '0';
        end if;
	end process;
end Behavioral;
```

I was curious about this part, so this would be a quick easy first thing to check.
```bash
architecture Behavioral of backdoor is
    constant pattern : STD_LOGIC_VECTOR(15 downto 0) := "1111111111101001";
```
---

## Exploitation
I connected to the container using the nc command provided, and inputed the 16-bit binary given "1111111111101001," and the output returned was the flag!

## Takeaways & Lessons Learned
I'm getting better at reconnaissance, specifically within CTF environments. There are usually enough clues given to pinpoint where to search for vulnernabilities, and this saves much time when solving the problem. While some challenges may be black-box environments even, there are still strong recon methodologies to follow to rule out the most common vulerabilities.