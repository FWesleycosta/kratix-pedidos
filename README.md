# kratix-pedidos

Pedidos (Resource Requests do Kratix) do ensaio pessoal Backstage + Kratix.
O Argo CD aplica o que está em `pedidos/` no cluster `kind-plataforma`, e o Kratix
processa cada pedido com o Promise correspondente.

## Como um pedido anda

1. O pedido entra como um arquivo YAML em `pedidos/<namespace>/<nome>.yaml`.
   A partir do passo 6 do ensaio, quem grava é o Scaffolder do Backstage. Até lá, a gravação é manual, para teste.
2. O Argo CD (Application `kratix-pedidos`, AppProject `kratix-pedidos`) aplica o arquivo no cluster.
3. O Kratix roda o Pipeline do Promise e grava o resultado no
   [kratix-state-store](https://github.com/FWesleycosta/kratix-state-store), que o Argo CD também aplica.

## Regras

- O commit vai direto na `main`, sem PR e sem aprovação: quem valida o pedido é o schema do Promise.
- Só entram recursos com namespace, e só no namespace `default` (limite do AppProject).
  Recurso de cluster é recusado pelo Argo CD.
- Repositório público: nada de Secret, senha, token ou dado sensível nos pedidos.
- Apagar o arquivo apaga o pedido no cluster (sync com `prune`), e o Kratix remove o que tinha gerado.
- Só o que está em `pedidos/` vai para o cluster. Este README e outros arquivos na raiz ficam de fora.

## Proteção

Ruleset na `main`: bloqueia force push e exclusão da branch.

## Onde está o resto

O bootstrap do cluster, do Argo CD e do Kratix está em
[kratix-plataforma](https://github.com/FWesleycosta/kratix-plataforma).
