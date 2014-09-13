#Maintaner: Yahya Mohajer <yaya_2013 {AT} yahoo {DOT} com >

pkgname=sonarqube
pkgver=4.4
pkgrel=1
pkgdesc="A code quality management platform."
url="http://sonar.codehaus.org"
arch=('i686' 'x86_64')
license=('GPL3')
depends=('java-environment')
optdepends=('apache: a full featured webserver'
            'mysql: A fast SQL database server'
            'maven: a java project management and project comprehension tool')

backup=('opt/sonarqube/conf/sonar.properties' 
        'opt/sonarqube/conf/wrapper.conf')

install=${pkgname}.install
conflicts=('java-sonar')
provides=('java-sonar' 'sonar')
options=(!strip)

source=(http://dist.sonar.codehaus.org/${pkgname}-${pkgver}.zip
        'sonar.sh'
        'wrapper.conf'
        'sonar.service')	

md5sums=('e053aa73e011df9ad6931aaa15380d4f'
         '44223ddba1c7394a25c8ab0755aeabad'
         'f4b12f47f0078b569c61ab41ee556ad5'
         'd8487480f1e5fb916309f60034453708')

build() {
  cd ${srcdir}

  # Create directory and copy everything
  install -d ${pkgdir}/opt/${pkgname}

  # moving only $CARCH specific files to pkg, delete the rest
  msg "Determine right architecture"
  if [ $CARCH = 'x86_64' ]; then
    cp -r ${srcdir}/${pkgname}-${pkgver}/bin/linux-x86-64 ${pkgdir}/opt/${pkgname}/bin || return 1
    rm -r ${srcdir}/${pkgname}-${pkgver}/bin || return 1
  elif [ $CARCH = 'i686' ]; then
    cp -r ${srcdir}/${pkgname}-${pkgver}/bin/linux-x86-32 ${pkgdir}/opt/${pkgname}/bin || return 1
    rm -r ${srcdir}/${pkgname}-${pkgver}/bin || return 1
  fi

  # install the additional config files to the desired destination
  msg "Installing configuration files"
  mkdir -p ${pkgdir}/opt/${pkgname}/conf
  install ${srcdir}/${pkgname}-${pkgver}/conf/sonar.properties ${pkgdir}/opt/${pkgname}/conf/sonar.properties
  install ${srcdir}/wrapper.conf ${pkgdir}/opt/${pkgname}/conf/wrapper.conf || return 1
  rm -r ${srcdir}/${pkgname}-${pkgver}/conf
  

  # copy documentation
  msg "Copy documentation"
  mkdir -p ${pkgdir}/usr/share/doc/${pkgname}/
  install ${srcdir}/${pkgname}-${pkgver}/COPYING ${pkgdir}/usr/share/doc/${pkgname}
  rm ${srcdir}/${pkgname}-${pkgver}/COPYING

  # delete not needed directories
  rm -r ${srcdir}/${pkgname}-${pkgver}/logs
  # link log directory
  ln -s /var/log/${pkgname} ${pkgdir}/opt/${pkgname}/logs

  # copy the source to the final directory
  msg "Copy Source to final directory"
  cp -a ${srcdir}/${pkgname}-${pkgver}/* ${pkgdir}/opt/${pkgname} || return 1

  install ${srcdir}/sonar.sh ${pkgdir}/opt/${pkgname}/bin/sonar.sh || return 1

  mkdir -p ${pkgdir}/var/log/${pkgname}/


  install -m755 -d ${pkgdir}/opt/${pkgname}/run

  install -Dm644 "${srcdir}/sonar.service" "${pkgdir}/usr/lib/systemd/system/sonar.service"
}

