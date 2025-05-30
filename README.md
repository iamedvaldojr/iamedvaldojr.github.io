Atualize o sistema::
apt update
apt upgrade

Instale as dependencias necessárias:
apt-get install ruby-full build-essential zlib1g-dev

Configure o ambiente 
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

Instale Jekell e Bundler:
gem install jekyll bundler

Clone o repositório:
git clone https://github.com/iamedvaldojr/iamedvaldojr.github.io.git
cd iamedvaldojr.github.io.git

Instale as dependencias do projeto:
bundle install

Inicie o servidor Jekyll:
bundle exec jekyll serve 0.0.0.0:4000 --livereload

Acesse via navegador:
127.0.0.1:4000 ou 168.168.XXX.XXX:4000

E também a página de admin:
127.0.0.1:4000/admin ou 168.168.XXX.XXX:4000/admin
