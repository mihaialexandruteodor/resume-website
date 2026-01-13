# resume-website

Github pages resume website

# 1. Install Ruby and system build tools

```zsh
sudo dnf install ruby ruby-devel openssl-devel gcc gcc-c++ make
```

# 2. Configure Ruby Gems to install to your home directory (avoids using sudo)

# This adds the gem binary path to your .zshrc (since we switched you to zsh earlier)

```zsh
echo '# Install Ruby Gems to ~/gems' >> ~/.zshrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.zshrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

# 3. Install Jekyll and Bundler

```zsh
gem install jekyll bundler
```

# 4. Navigate to your project folder and install dependencies

# Run these inside your website directory

```zsh
bundle install
```

# 5. Start the server

```zsh
bundle exec jekyll serve --host 0.0.0.0 --watch
```
