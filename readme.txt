# Install OSX Developer Tools
xcode-select --install

# Install Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"

# Update Homebrew
brew update

# If you get a " /usr/local must be writable error ", just run this:
sudo chown -R $USER:admin /usr/local
brew update

# Install some dependencies
function install_or_upgrade { brew ls | grep $1 > /dev/null; if (($? == 0)); then brew upgrade $1; else brew install $1; fi }
install_or_upgrade "git"
install_or_upgrade "wget"
install_or_upgrade "imagemagick"
install_or_upgrade "jq"
install_or_upgrade "openssl"

# Install Oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

# Quit and restart terminal

# Open terminal, go to preferences, select the "Pro" profile, and set as default

# Generate SSH key for GitHub
mkdir -p ~/.ssh && ssh-keygen -t ed25519 -o -a 100 -f ~/.ssh/id_ed25519 -C "TYPE_YOUR_EMAIL@HERE.com"

# View public key
cat ~/.ssh/id_ed25519.pub

# Go to GitHub settings in browser, click "New SSH Key",
# paste contents of public key, click "Add SSH Key"
https://github.com/settings/ssh

# To check the above steps are completed, run
ssh -T git@github.com

# Expect to see "Hi --------! You've successfully authenticated..."
# If not, run this before trying  ssh -T git@github.com  again
ssh-add ~/.ssh/id_ed25519

# Set variable to hold GitHub username
export GITHUB_USERNAME=replace_this_with_your_github_username

# Clone dotfiles configuration repo
mkdir -p ~/code/$GITHUB_USERNAME && cd $_ && git clone git@github.com:$GITHUB_USERNAME/dotfiles.git

# Run the dotfiles installer
cd ~/code/$GITHUB_USERNAME/dotfiles
zsh install.sh

# Run the git installer
cd ~/code/$GITHUB_USERNAME/dotfiles
zsh git_setup.sh

# Provide name and GitHub email address

# Quit terminal

# Store SSH passphrase in keychain
touch ~/.ssh/config
st ~/.ssh/config

# Add these lines to SSH config file and save
Host *
  AddKeysToAgent yes
  UseKeychain yes

# Clean up previous Ruby installations
rvm implode && sudo rm -rf ~/.rvm

# Expect to see "zsh: command not found: rvm"
# It means rvm is not installed, which is OK.

# Then run
sudo rm -rf $HOME/.rbenv /usr/local/rbenv /opt/rbenv /usr/local/opt/rbenv

# Install rbenv and ruby-build
brew uninstall --force rbenv ruby-build

# Quit terminal

# Run
brew install rbenv

# Quit terminal

# Install latest version of Ruby (2.6.6 at time of writing)
rbenv install 2.6.6

# Tell system to use version 2.6.6 by default
rbenv global 2.6.6

# Quit terminal

# Check Ruby version
ruby -v

# Install some gems
gem install rake bundler rspec rubocop rubocop-performance pry pry-byebug hub colored octokit

# If you see "ERROR: While executing gem ... (TypeError) incompatible
# marshal file format (can't be read) format version 4.8 required;
# 60.33 given", run this before trying the command to install gems again
rm -rf ~/.gemrc

# Never install a gem with  sudo gem install  even if terminal says so

# Install postgresql
brew install postgresql
brew services start postgresql

# Check if postgresql installed successfully
psql -d postgres

# If you enter a new postgres prompt, that is good.
# Type \q to exit

# Check all is installed
curl -Ls https://raw.githubusercontent.com/lewagon/setup/master/check.rb > _.rb && ruby _.rb || rm _.rb







