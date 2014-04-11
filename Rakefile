ENV['PATH'] = "/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"

HP, HP_VERSION = 'haskell-platform', '2013.2.0.0'
HP_DIR = "#{HP}-#{HP_VERSION}"

HTAR = 'hp.tar.bz2'
HP_TAR = "/var/tmp/#{HTAR}"

desc "build and install ghc and haskell-platform"
task :all do
  begin
    if 'Linux' == uname
      link_libgmp = '/usr/lib/libgmp.so.3'
      libgmp = '/usr/lib/x86_64-linux-gnu/libgmp.so'
      sh "sudo ln -fs #{libgmp} #{link_libgmp}"
    end
    [:libs, :ghc, :hp].each do |t|
      task(t).reenable
      task(t).invoke
    end
  ensure
    sh "sudo rm -f #{link_libgmp}"
    raise $!
  end
end

desc "build and install #{HP}-#{HP_VERSION}"
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

GHC, GHC_VERSION = 'ghc', '7.8.1'
GHC_DIR = "#{GHC}-#{GHC_VERSION}"

GTAR = 'ghc.tar.bz2'
GHC_TAR = "/var/tmp/#{GTAR}"

desc "build and install #{GHC}-#{GHC_VERSION}"
task :ghc => GHC_DIR do
  if 'Linux' == uname
    Dir.chdir(GHC_DIR) do
      sh "./configure --prefix=/opt/hp"
      sh "cd . && make"
      sh "cd . && sudo make install"
    end
  end
end

directory GHC_DIR => GHC_TAR do
    sh "tar xf #{GHC_TAR}"
end

# http://www.haskell.org/ghc/dist/7.8.1/ghc-7.8.1-src.tar.bz2
file GHC_TAR do
    url = "http://www.haskell.org/ghc/dist/#{GHC_VERSION}/ghc-#{GHC_VERSION}-src.tar.bz2"
    sh "wget #{url} -O #{GHC_TAR}"
end

desc "build ghc from its git repo"
task :ghc_git do
  ENV['PATH'] = "/usr/local/bin:/usr/bin:/bin"
  unless Dir.exists?('/var/tmp/ghc')
    Dir.chdir('/var/tmp') do
      sh "git clone http://git.haskell.org/ghc.git" 
    end
  end
  Dir.chdir('/var/tmp/ghc') do 
    sh "git pull"
    sh "./sync-all get"
    sh "sudo aptitude update -y"
    sh "sudo aptitude install -y libxml2-dev libxml2-utils libxslt-dev libxslt-utils autoconf ghc happy alex cabal-install"
    sh "perl boot"
    sh "./configure --prefix=/opt/hp"
    sh "make"
    sh "make install"
  end
end

task :libs do
  if 'Linux' == uname
    ls = [] << 'libgmp-dev'
    ls << 'libgmp3-dev'
    ls << 'zlib1g-dev'
    ls << 'libgl1-mesa-dev'
    ls << 'libglc-dev'
    ls << 'freeglut3-dev'
    ls << 'libedit-dev'
    ls << 'libglw1-mesa-dev'
    ls << 'libglw1-mesa'
    sh "sudo aptitude update -y"
    ls.each{|l|sh "sudo aptitude install -y #{l}"}
  end
end

task :clean do
    sh "sudo rm -rf /var/tmp/ghc.* ghc-*"
    sh "sudo rm -rf /var/tmp/hp.* haskell-*"
end

def uname
  `uname -s`.strip
end
