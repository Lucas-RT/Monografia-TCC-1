#$Header: /home/cvsroot/abntex/Makefile,v 1.30.2.1 2004/11/02 16:18:13 gweber Exp $
#$Log: Makefile,v $
#Revision 1.30.2.1  2004/11/02 16:18:13  gweber
#addicionado --dereference em install: para corrigir bug 212
#
#Revision 1.30  2003/10/17 14:21:09  gweber
#Mais uma pequenca corre??o para gerar abntex.spec
#
#Revision 1.29  2003/10/17 13:45:18  gweber
#abntex.spec ? criado a partir de admin/abntex.spec substituindo
#as vari?veis para version/release.
#
#Revision 1.28  2003/07/01 10:18:48  gweber
#Corre??o no conectiva-rpm
#
#Revision 1.27  2003/07/01 09:16:58  gweber
#Adicionado check-rpm-install
#
#Revision 1.26  2003/04/24 23:26:15  gweber
#adicionadas listas de exclus?o para gera??o do pacote contrib
#
#Revision 1.25  2003/04/10 11:31:27  gweber
#Cria??o de pacotes abntex-contrib
#
#Revision 1.24  2003/03/31 21:10:50  gweber
#consertado um } que faltou
#
#Revision 1.23  2003/03/31 18:34:47  gweber
#css s?o copiadas em vez de linkadas
#
#

#altere a linha abaixo se o seu tetex n?o puder se detetado
#pelo comando kpsewich, na d?vida fa?a um "make test"
#TEXDIR= /tmp/test
TEXDIR= ${shell kpsewhich --expand-path='$$TEXMFMAIN'}

USRLOCAL=/usr/local/bin

#Do "man lyx":
#The system directory is determined by  searching  for  the
#       file  "chkconfig.ltx".
LYXDIR=${dir ${shell  find /usr/share/lyx -name chkconfig.ltx}}

#diret?rio corrente onde se expandiu o cvs do abntex
ABNTEXDIR=$(notdir $(shell pwd))

#diret?rio onde se encontra a documenta??o original
SOURCEDOC=$(shell pwd)/texmf/doc

#diret?rio para onde vai a documenta??o compilada
CDOCDIR=$(shell pwd)/compiled.docs

#diret?rio para onde est?o as contribui??es
CONTRIBDIR=$(shell pwd)/contrib

#script para gerar tabela de s?mbolos
GERATSS=$(shell pwd)/bin/geratss

#diret?rio onde se encontram os arquivos da homepage
HPDIR =pagina

#email da pessoa que envia os arquivos pro codigolivre
#edite se precisar
SUBMITTER=$(shell logname)@$(shell hostname)

#op??es para comandos comuns:
OPS = --symbolic
OPSF= --symbolic --force
OPVR= --verbose --recursive
OPRF= --recursive --force
OPVF= --verbose --force
#op??es comuns para tar
OPTAR= --owner=0 --group=0 -zchv

LATEXDOCDIR=${SOURCEDOC}/latex/abntex
BIBTEXDOCDIR=${SOURCEDOC}/bibtex/abntex

LATEXSDOCS=${wildcard ${LATEXDOCDIR}/abnt*.tex} ${wildcard ${LATEXDOCDIR}/tab*.tex}

LATEXSCSS=${wildcard ${LATEXDOCDIR}/*.css}
BIBTEXSDOCS=${wildcard ${BIBTEXDOCDIR}/*.tex}
BIBTEXSCSS=${wildcard ${BIBTEXDOCDIR}/*.css}

LATEXDOCS=${subst ${LATEXDOCDIR},${CDOCDIR},${LATEXSDOCS}}
LATEXCSS=${subst ${LATEXDOCDIR},${CDOCDIR},${LATEXSCSS}}

BIBTEXDOCS=${subst ${BIBTEXDOCDIR},${CDOCDIR},${BIBTEXSDOCS}}
BIBTEXCSS=${subst ${BIBTEXDOCDIR},${CDOCDIR},${BIBTEXSCSS}}

ALLSDOCS=${BIBTEXSDOCS} ${LATEXSDOCS}
ALLDOCS=${BIBTEXDOCS} ${LATEXDOCS}
ALLCSS=${BIBTEXCSS} ${LATEXCSS}

DVIDOCS=${ALLDOCS:.tex=.dvi}
PDFDOCS=${ALLDOCS:.tex=.pdf}
PSGZDOCS=${ALLDOCS:.tex=.ps.gz}
HTMLDOCS=${ALLDOCS:.tex=.html}

#vari?veis de ambiente que precisam ser exportados para rodar a
#documenta??o corretamente
export TEXINPUTS=$(shell pwd)/texmf/tex/latex//:
export BIBINPUTS=$(shell pwd)/texmf/bibtex/bib//:
export BSTINPUTS=$(shell pwd)/texmf/bibtex/bst//:
export INDEXSTYLE=$(shell pwd)/texmf/tex/latex//:

homepage:
	cd ${HPDIR}; latex pagina; bibtex pagina;\
	cp pagina.bbl pagina.bbx; \
	sed "/bibitem/s/{ISO.*\|{NBR.*/[&]&/g" pagina.bbx > pagina.bbl;\
	latex pagina; latex pagina;\
	latex2html pagina

homepage-clean:
	cd ${HPDIR}; rm -rf *.aux *.blg  *.bbl  *.bbx *.dvi *.log *.pl;\
	rm -rf img*.png node*.html pagina.html index.html WARNINGS footnote.html


#aqui nos alteramos a vari?vel TEX da regra impl?cita %.dvi:%.tex
#para as particularidades de cada documento
${CDOCDIR}/abnt-bibtex-doc.dvi: TEX=${LBLBLL}
${CDOCDIR}/abnt-bibtex-doc.pdf: TEX=${LBLBLL}
${CDOCDIR}/abnt-bibtex-alf-doc.dvi: TEX=${LBLL}
${CDOCDIR}/abnt-bibtex-alf-doc.pdf: TEX=${LBLL}
${CDOCDIR}/abnt-classe-doc.dvi: TEX=${LLL}
${CDOCDIR}/abnt-classe-doc.pdf: TEX=${LLL}
${CDOCDIR}/tabela-simbolos-doc.dvi: TEX=${LTBLL}
${CDOCDIR}/tabela-simbolos-doc.pdf: TEX=${LTBLL}
${CDOCDIR}/inpi-ex.dvi: TEX=${LLL}

#expande o nome do arquivo
FILE=	$(basename $(notdir $<))

#expande o comando de mudan?a de diret?rio
CDDIR=  cd $(dir $<);

#expande a vers?o deste pacote
VERSION= $(shell cat abntex_version)
RELEASE= $(shell cat abntex_release)
CONTRIBVERSION= $(shell cat contrib/contrib_version)

#arquivos dos pacotes
ABNTEX=abntex-${VERSION}-${RELEASE}
ABNTEXDOC=${subst abntex,abntex-doc,${ABNTEX}}
ABNTEXLYX=${subst abntex,abntex-lyx,${ABNTEX}}
ABNTEXTGZ=${ABNTEX}.tgz
ABNTEXDOCTGZ=${ABNTEXDOC}.tgz
ABNTEXZIP=${ABNTEX}.zip
ABNTEXDOCZIP=${ABNTEXDOC}.zip
ABNTEXSRCRPM=${ABNTEX}.src.rpm
ABNTEXRPM=${ABNTEX}.noarch.rpm
ABNTEXDOCRPM=${ABNTEXDOC}.noarch.rpm
ABNTEXLYXRPM=${ABNTEXLYX}.noarch.rpm

CONTRIB=abntex-contrib-${CONTRIBVERSION}
CONTRIBTGZ=${CONTRIB}.tgz
CONTRIBZIP=${CONTRIB}.zip

#diret?rios padr?o do rpm (Conectiva)
RPMS=/usr/src/rpm/RPMS/noarch
SRPMS=/usr/src/rpm/SRPMS
SOURCES=/usr/src/rpm/SOURCES

#diret?rios padr?o do rpm (SuSE)
RPMS=/usr/src/packages/RPMS/noarch
SRPMS=/usr/src/packages/SRPMS
SOURCES=/usr/src/packages/SOURCES

#diret?rio para gera??o de pacotes
PACKDIR=abntex-${VERSION}

#diret?rio para gera??o de pacotes contribuidos
CONTPACKDIR=abntex-contrib-${CONTRIBVERSION}


LATEX=latex

#roda latex e bibtex de modo a resolver todas as refer?ncias
LBLBLL= cd ${CDOCDIR}; ${LATEX} ${FILE};\
	bibtex ${FILE}; ${LATEX} ${FILE}; \
	bibtex ${FILE}; ${LATEX} ${FILE}; latex ${FILE}

#documento com bibliografia simples
LBLL= cd ${CDOCDIR}; ${LATEX} ${FILE};\
	bibtex ${FILE}; ${LATEX} ${FILE}; ${LATEX} ${FILE}

#documento com bibliografia e lista de tabelas
LTBLL= cd ${CDOCDIR}; ${LATEX} ${FILE};\
	${GERATSS} ${FILE}; bibtex ${FILE};\
	${LATEX} ${FILE}; ${GERATSS} ${FILE}; ${LATEX} ${FILE}

#documento simples
LLL= cd ${CDOCDIR}; ${LATEX} ${FILE}; ${LATEX} ${FILE}; ${LATEX} ${FILE}

${CDOCDIR}:
	mkdir ${CDOCDIR}

${BIBTEXDOCS} ${BIBTEXCSS}: ${CDOCDIR}
	cp ${OPF} ${BIBTEXDOCDIR}/$(notdir $@) $@

${LATEXDOCS} ${LATEXCSS}: ${CDOCDIR}
	ln ${OPSF} ${LATEXDOCDIR}/$(notdir $@) $@

#depend?ncia entre dvi<->ps
%.ps: %.dvi
	cd ${CDOCDIR}; \
	dvips -t a4 -o ${FILE}.ps ${FILE}

#compress?o do arquivo ps
%.ps.gz: %.ps
	cd ${CDOCDIR}; gzip --force $<

#depend?ncia entre pdf<->tex
%.pdf: %.tex
	$(subst latex,pdflatex,${TEX})$

HTMLOPT= -split 0 -noinfo -noaddress -html_version 4.0 -no_navigation

%.html: %.dvi
	${CDDIR} latex2html ${HTMLOPT} ${FILE}; \
	ln ${OPSF} ${FILE}/index.html ${FILE}.html

#doc-clean remove arquivos intermedi?rios desnecess?rios (exceto *.dvi)
#em compiled.docs
doc-clean: ${CDOCDIR}
	cd ${CDOCDIR};\
	rm ${OPVF} *.log *.blg *.bbl *.aux *.toc *.idx *.los *.lot *.out *.ilg\
	*.sigla* *.symbols*

#com clean somente os arquivos fonte sobram
clean:
	rm ${OPRF} ${CDOCDIR}

doc-remove-links:
	cd ${CDOCDIR};\
	rm ${OPVF} *.tex *.css


all:
	make doc

doc:
	make doc-clean
	make doc-dvi
	make doc-ps
	make doc-html
	make doc-clean
	make doc-pdf
	make doc-clean
	make doc-remove-links

#cria links dos fontes da documenta??o para a cria??o de
#documenta??o compilada no diret?rio CDOCDIR
doc-links: ${ALLDOCS} ${ALLCSS}

doc-dvi: ${DVIDOCS}

doc-pdf: ${PDFDOCS}

doc-ps:	 ${PSGZDOCS}

doc-html: ${HTMLDOCS}

#O empacotamento para Linux

PACKLIST= ${PACKDIR}/Makefile \
	  ${PACKDIR}/abntex_version ${PACKDIR}/abntex_release \
	  ${PACKDIR}/*.lst \
	  ${PACKDIR}/LEIAME ${PACKDIR}/LEIAME.make ${PACKDIR}/LEIAME.linux \
	  ${PACKDIR}/LEIAME.administracao \
	  ${PACKDIR}/texmf ${PACKDIR}/bin ${PACKDIR}/lyx

DOCLIST = ${PACKDIR}/$(notdir ${CDOCDIR})

#principal pacote que cont?m os programas fontes do abnTeX
linux-tgz:
	cd ..; \
	ln ${OPSF} ${ABNTEXDIR} ${PACKDIR}; \
	rm ${OPVR} ${ABNTEXTGZ};\
	tar ${OPTAR}  --exclude-from ${ABNTEXDIR}/exc.lst -f ${ABNTEXTGZ} ${PACKLIST};\
	rm ${PACKDIR}

contrib-tgz:
	cd ..; \
	ln ${OPSF} ${CONTRIBDIR} ${CONTPACKDIR};\
	rm ${OPRF} ${CONTRIBTGZ};\
	tar ${OPTAR}  --exclude-from ${CONTPACKDIR}/exc.lst -f ${CONTRIBTGZ} ${CONTPACKDIR};\
	rm ${CONTPACKDIR}

#pacote opcional com a documenta??o precompilada em dvi, pdf, ps e html
linux-doc-tgz:
	make doc
	cd ..; \
	ln ${OPSF} ${ABNTEXDIR} ${PACKDIR}; \
	rm ${OPVR} ${ABNTEXDOCTGZ};\
	tar ${OPTAR} -f ${ABNTEXDOCTGZ} ${DOCLIST};\
	rm ${PACKDIR}

#install e uninstall espec?fico para tetex localizado em ${TEXDIR}
install:
	cp ${OPVR} --dereference --preserve texmf/* ${TEXDIR}
	cp ${OPVR} --dereference --preserve lyx/* ${LYXDIR}
	cp ${OPVR} --dereference --preserve bin/* ${USRLOCAL}
	texhash

#instala??o m?nima a partir de um diret?rio cvs/abntex ?til para testes
#n?o inclui documenta??o
install-from-cvs:
	cp ${OPVR} --parents texmf/bibtex/*/abntex/*.b* $(TEXDIR)/..
	cp ${OPVR} --parents texmf/tex/latex/abntex/*.cls $(TEXDIR)/..
	cp ${OPVR} --parents texmf/tex/latex/abntex/*.sty $(TEXDIR)/..
	texhash

uninstall:
	rm ${OPRF} ${TEXDIR}/doc/latex/abntex;\
	rm ${OPRF} ${TEXDIR}/doc/bibtex/abntex
	rm ${OPRF} ${TEXDIR}/bibtex/bst/abntex;\
	rm ${OPRF} ${TEXDIR}/bibtex/bib/abntex;\
	rm ${OPRF} ${TEXDIR}/tex/latex/abntex;\
	rm ${OPRF} ${LYXDIR}abnt.layout;\
	rm ${OPRF} ${USRLOCAL}/geratss
	texhash

#verifica se todos os pacotes necess?rios existem no seu sistema
check-rpm-install:
	rpm -q cvs
	rpm -q rpm-build
	rpm -q latex2html perl perl-modules
	rpm -q lyx

#para montar pacotes tipo rpm ser? melhor voc? ser root.
conectiva-rpm:
	if test "$(shell whoami)" == "root" ; then \
	cp ../${ABNTEXTGZ} ${SOURCES};\
	sed "s/ABNTEXVERSION/${VERSION}/;s/ABNTEXRELEASE/${RELEASE}/" admin/abntex.spec > abntex.spec;\
	rpmbuild -ba --target=noarch --clean abntex.spec;\
	else\
	echo "you should be root to do this";\
	fi

windows-zip:
	cd ../; \
	rm ${OPVF} ${ABNTEXZIP};\
	ln ${OPSF} ${ABNTEXDIR} ${PACKDIR}; \
	zip -r ${ABNTEXZIP} ${PACKDIR} -x@${ABNTEXDIR}/zipexcl.lst;\
	rm ${PACKDIR}

windows-doc-zip:
	make doc
	cd ../; \
	rm ${OPVF} ${ABNTEXDOCZIP};\
	ln ${OPSF} ${ABNTEXDIR} ${PACKDIR}; \
	zip -r ${ABNTEXDOCZIP} ${PACKDIR}/compiled.docs;\
	rm ${PACKDIR}

contrib-zip:
	cd ..; \
	ln ${OPSF} ${CONTRIBDIR} ${CONTPACKDIR};\
	rm ${OPRF} ${CONTRIBZIP};\
	zip -r ${CONTRIBZIP} ${CONTPACKDIR} -x@${CONTPACKDIR}/zipexcl.lst;\
	rm ${CONTPACKDIR}

new-release:
	make clean
	make doc
	make linux-tgz
	make linux-doc-tgz
	make windows-zip
	make windows-doc-zip
	make conectiva-rpm

new-contrib:
	make contrib-tgz
	make contrib-zip

upload:
	echo "open upload.codigolivre.org.br" > upload.ftp;\
	echo "user anonymous ${SUBMITTER}" >> upload.ftp;\
	echo "cd incoming" >> upload.ftp;\
	echo "passive" >> upload.ftp;\
	echo "put ../${ABNTEXTGZ} ${ABNTEXTGZ}" >> upload.ftp;\
	echo "put ../${ABNTEXDOCTGZ} ${ABNTEXDOCTGZ}" >> upload.ftp;\
	echo "put ../${ABNTEXZIP} ${ABNTEXZIP}" >> upload.ftp;\
	echo "put ../${ABNTEXDOCZIP} ${ABNTEXDOCZIP}" >> upload.ftp;\
	echo "put ${SRPMS}/${ABNTEXSRCRPM} ${ABNTEXSRCRPM}" >> upload.ftp;\
	echo "put ${RPMS}/${ABNTEXRPM} ${ABNTEXRPM}" >> upload.ftp;\
	echo "put ${RPMS}/${ABNTEXDOCRPM} ${ABNTEXDOCRPM}" >> upload.ftp;\
	echo "put ${RPMS}/${ABNTEXLYXRPM} ${ABNTEXLYXRPM}" >> upload.ftp;\
	echo "quit" >> upload.ftp
	ftp -v -n < upload.ftp
	rm upload.ftp

upload-contrib:
	echo "open upload.codigolivre.org.br" > upload.ftp;\
	echo "user anonymous ${SUBMITTER}" >> upload.ftp;\
	echo "cd incoming" >> upload.ftp;\
	echo "passive" >> upload.ftp;\
	echo "put ../${CONTRIBTGZ} ${CONTRIBTGZ}" >> upload.ftp;\
	echo "put ../${CONTRIBZIP} ${CONTRIBZIP}" >> upload.ftp;\
	echo "quit" >> upload.ftp
	ftp -v -n < upload.ftp
	rm upload.ftp


test:
	echo "diret?rio corrente:" ${ABNTEXDIR}
	echo "caminho para o tetex:" ${TEXDIR}
	echo "caminho para o LyX:" ${LYXDIR}
	echo "${LATEXDOCS}"

