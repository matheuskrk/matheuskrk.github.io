# site/ — as quatro exigências da TikTok, num lugar só

A TikTok não registra app sem site. Para app criado depois de 09/09/2024 ela
exige **URL do site, Termos de Uso e Política de Privacidade — todos
verificados** — e o redirect do OAuth tem que ser **https**, sem localhost
("URIs must be absolute and begin with https"). São quatro coisas, e estas
quatro páginas resolvem as quatro.

```
index.html             o site (o que a ferramenta faz, e o que não faz)
privacidade.html       Política de Privacidade
termos.html            Termos de Uso
tiktok-callback.html   o redirect https — mostra o code para você colar
estilo.css
```

## Manutenção

Operador e contato estão no rodapé das três páginas; a data de vigência, no
topo de `privacidade.html` e `termos.html`. Mudou alguma coisa no que a
ferramenta faz, atualize o texto **e** a data — é o texto que a revisão da
TikTok confere.

Releia o texto a cada mudança de escopo. Ele descreve o uso real (ler métricas das
próprias contas, guardar local, não compartilhar com ninguém) e precisa
continuar verdadeiro — é isso que a revisão da TikTok confere.

## Onde hospedar

**GitHub Pages**, e por um motivo específico: a verificação da TikTok pede
um arquivo de assinatura na **raiz** do domínio. Num site de usuário
(`usuario.github.io`) você controla a raiz; numa página de projeto
(`usuario.github.io/repo/`) não, e a verificação falha.

```bash
cd site
git init && git add -A && git commit -m "site"
git branch -M main
git remote add origin https://github.com/<usuario>/<usuario>.github.io.git
git push -u origin main
```

Em Settings → Pages, aponte para a branch `main`. Em poucos minutos o site
está em `https://<usuario>.github.io/`, com HTTPS.

Cloudflare Pages e Netlify servem igual e ainda aceitam verificação por DNS.
Domínio próprio (~R$40/ano) é a opção que vale se você quiser o nome da marca
aparecendo — e serve às três páginas de uma vez.

Não dá para usar um link do Notion ou do Google Docs: sem controle da raiz,
o arquivo de verificação não sobe.

## Depois de publicar

1. developers.tiktok.com → Manage apps → app novo
2. em **URL properties**, verifique o domínio (baixe o arquivo de assinatura
   que eles dão, coloque na raiz do repositório, commit, push, verifique)
3. preencha site, termos e privacidade com as três URLs
4. produto **Login Kit**, escopo `video.list`
5. Redirect URI: `https://<seu-site>/tiktok-callback.html`
6. grave em `.segredos/tiktok-lagosta-fit-br.json`:

```json
{
  "client_key": "...",
  "client_secret": "...",
  "redirect_uri": "https://<seu-site>/tiktok-callback.html"
}
```

Em **sandbox** o app já funciona com a sua própria conta, sem esperar
revisão — que é exatamente o caso de uso aqui.
