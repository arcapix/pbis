�����^�񓯊��^�z�b�v���^�]���p�X�w�� �l�����Z�T���v��
=====================================================

���̃f�B���N�g���ɂ́A�l�����Z�T�[�r�X�ƁAOpenSOAP�T�[�o�̃��b�Z�[�W��
�O�@�\�i�����^�񓯊��y�сA�T�[�o�]�����̍ő�z�b�v���Ɠ]���z�X�g���w��j
�𗘗p�ł���N���C�A���g�̃T���v���R�[�h���܂܂�Ă��܂��B
�N���C�A���g�ɂ̓R�}���h���C���ł�Web�u���E�U�ł�����܂��B

OpenSOAP API �C���X�g�[����́Amake -f CalcAsync.mak �ŁA
�R���p�C���ł��A�ȉ��̃t�@�C�����쐬����܂��B

* CalcAsyncClient
  �R�}���h���C���ł̎l�����Z�N���C�A���g�v���O�����ł��B
* CalcAsyncApp.cgi
  CGI �^�̎l�����Z�N���C�A���g�v���O�����ł��BWeb�u���E�U���
  ���s���邱�Ƃ��\�ł��B
* CalcAsyncService
  �W�����o�͂�p�����l�����Z�T�[�r�X�v���O�����ł��B
  inetd ��p���邱�ƂŁAsocket type �̃T�[�r�X�ɂ��邱�Ƃ��\�ł��B
* CalcAsyncService.cgi
  CGI �^�̎l�����Z�T�[�r�X�v���O�����ł��B

���[�g�����ŁAmake -f CalcAsync.mak install �����s����ƁA�T�[�r�X�v��
�O������ SSML�t�@�C���A�y�� �N���C�A���g�v���O�������C���X�g�[�����܂��B
�V�K��SSML�t�@�C�����C���X�g�[�������ꍇ�ɂ́A
# opensoap-server-ctl reload
�ŁA�ݒ�t�@�C����ǂݒ����ĉ������B


�R�}���h���C���� CalcAsyncClient �̎g�����͈ȉ��̂悤�ɂȂ�܂��B

Usage: CalcAsyncClient [options] method [op_a] [op_b]
  method         Add[Async], Subtract[Async], Multiply[Async], Divide[Async]
                 or GetResult.
  op_a           first operand (except for GetResult)
  op_b           second operand (except for GetResult)

 [options]
  -s endpoint    SOAP service endpoint URI
  -a soapaction  value of SOAPAction HTTP-Header
  -c char_enc    character encoding of request message (default: UTF-8)

  (messaging options)
  -H hopcount             set hopcount (>= 0)
  -f path1[,path2[,...]]  set forward path by URI (Commas are not allowed)
                          e.g.http://host/cgi-bin/soapInterface.cgi
  -F forward_path_file    set forward path from input file

op_a �� op_b ��2�̐����ƁA4��ނ̌v�Z���@�A
Add(�����Z) Subtract(�����Z) Multiply(�|���Z) Divide(����Z)
���w�肵�܂��B

$ ./CalcAsyncClient Add 1 2
  (�������[�h�� 1 + 2 ���v�Z)

�T�[�o�̔񓯊����[�h�Ŏ��s����ꍇ�́A�v�Z���@�� Async �����܂��B
�񓯊��ł͌v�Z���ʂ͂����ɂ͕Ԃ炸�A�T�[�o���ŏ������s���A�ʓr������
�ʂ�₢���킹��K�v������܂��B

$ ./CalcAsyncClient MultiplyAsync 1 2 > tmp_msg
  (�񓯊����[�h�� 1 * 2 ���v�Z)
$ ./CalcAsyncClient GetResult < tmp_msg
  (�Ԃ���郁�b�Z�[�WID�����Ɍ��ʂ�₢���킹��)

�R�}���h���C���I�v�V�����ŃT�[�r�X�̃G���h�|�C���g���w��ł��܂��B
hostname���OpenSOAP�T�[�o�ŃT�[�r�X�����s����ꍇ�ɂ�
-s http://hostname/cgi-bin/soapInterface.cgi
�ƂȂ�܂��B

�w�肵��OpenSOAP�T�[�o��ɃT�[�r�X���Ȃ��ꍇ�A���̃T�[�o�͕ʂ̃T�[�o��
���b�Z�[�W��]�����ď������˗����܂��B
�ǂ��܂œ]�������邩�̍ő�z�b�v���� -H �I�v�V�����Ŏw��ł��܂��B

$ ./CalcAsyncClient -s http://host/cgi-bin/soapInterface.cgi -H 4 AddAsync 1 2
  (�T�[�o:host �ɃT�[�r�X���Ȃ��ꍇ�A4��܂œ]�������)

�]������T�[�o���N���C�A���g�����w�肷�邱�Ƃ��ł��܂��B
�t�@�C������ǂݍ���(-F)���A�R�}���h���C��(-f)����w��ł��܂��B

$ cat forward_path.txt
http://host1/cgi-bin/soapInterface.cgi
http://host2/cgi-bin/soapInterface.cgi

$ ./CalcAsyncClient -s http://host/cgi-bin/soapInterface.cgi \
                    -F forward_path.txt  AddAsync 1 2
  (�t�@�C������w��F�T�[�ohost�ɃT�[�r�X���Ȃ��ꍇ�Ahost1, host2 �Ə�
  �ɓ]�������)

$ ./CalcAsyncClient -s http://host/cgi-bin/soapInterface.cgi \
  -F http://host1/cgi-bin/soapInterface.cgi,http://host2/cgi-bin/soapInterface.cgi \
  AddAsync 1 2
  (�R�}���h���C���I�v�V�����Ŏw��F���l��host1, host2 �Ɠ]��)

CalcAsync.mak �ɊȒP�ȃe�X�g�p�̃��[�����p�ӂ��Ă���܂��B

$ make -f CalcAsync.mak stest1
  (�T���v�����N�G�X�g���b�Z�[�W���g�����T�[�r�X�@�\�̃e�X�g)
$ make -f CalcAsync.mak ctest1
  (�N���C�A���g�@�\�̃e�X�g)
$ make -f CalcAsync.mak cstest1
  (�T�[�r�X�E�N���C�A���g�@�\�̘A�g�̃e�X�g)
$ make -f CalcAsync.mak cstest1a
$ make -f CalcAsync.mak cstestr
  (�T�[�r�X�E�N���C�A���g�@�\�̔񓯊��ł̃e�X�g�B
   cstestr�ŏ������ʂ�₢���킹�ł��܂��B)


���l�̋@�\�� CGI�ł̃N���C�A���g���g���� Web�u���E�U��Ŏ��s�ł��܂��B
�f�t�H���g�ŃC���X�g�[�������ƁA����URL���珈�����s���܂��B
http://localhost/cgi-bin/CalcAsyncApp.cgi

�N���C�A���g�ő���M�����SOAP���b�Z�[�W�̓��e���m�F�������ꍇ�́A
"Display Request & Response messages" ���`�F�b�N���ĉ������B