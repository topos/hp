ENV['PATH'] = "/opt/hp/bin:#{ENV['PATH']}"

HP, HP_VERSION = 'haskell-platform', '2013.2.0.0'
HP_DIR = "#{HP}-#{HP_VERSION}"

TAR = 'hp.tar.bz2'
HP_TAR = "/var/tmp/#{TAR}"

task :hp => HP_DIR do
    Dir.chdir(HP_DIR) do
        sh "./configure --prefix=/opt/hp && sudo make"
    end
end

directory HP_DIR => HP_TAR do
    sh "tar xf #{HP_TAR}"
end

file HP_TAR do
    url = "http://lambda.haskell.org/platform/download/#{HP_VERSION}/haskell-platform-#{HP_VERSION}.tar.gz"
    sh "wget #{url} -O #{HP_TAR}"
end

GHC, GHC_VERSION = 'ghc', '7.6.3'
GHC_DIR = "#{GHC}-#{GHC_VERSION}"

TAR = 'ghc.tar.bz2'
GHC_TAR = "/var/tmp/#{TAR}"

task :ghc => GHC_DIR do
    Dir.chdir(GHC_DIR) do
        sh "./configure --prefix=/opt/hp && sudo make install"
    end
end

directory GHC_DIR => GHC_TAR do
    sh "tar xf #{GHC_TAR}"
end

file GHC_TAR do
    url = "http://www.haskell.org/ghc/dist/#{GHC_VERSION}/ghc-#{GHC_VERSION}-x86_64-unknown-linux.tar.bz2"
    sh "wget #{url} -O #{GHC_TAR}"
end

task :libs do
    ls = [] << 'libgmp-dev'
    ls << 'zlib1g-dev'
    ls << 'libgl1-mesa-dev'
    ls << 'libglc-dev'
    ls << 'freeglut3-dev'
    ls << 'libedit-dev'
    ls << 'libglw1-mesa-dev'
    ls << 'libglw1-mesa'

    sh "sudo aptitude update -y"
    ls .each{|l|sh "sudo aptitude install -y #{l}"}
end

task :clean do
    sh "sudo rm -rf /var/tmp/ghc.* ghc-*"
    sh "sudo rm -rf /var/tmp/hp.* haskell-*"
end