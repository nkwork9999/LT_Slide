git init
npm install -g @marp-team/marp-cli

chart を表示させる
npm install -g serve
serve out

npx @marp-team/marp-cli slide.md --html --template bespoke -o slide.html

marp test.md -o test.pdf --allow-local-files
