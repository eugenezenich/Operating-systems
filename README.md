# Operating-systems
Задача №1 (выбрал язык C++ и задачу с факториалом):

Программа:

<img width="903" height="438" alt="Снимок экрана 2026-05-20 173421" src="https://github.com/user-attachments/assets/227e3386-0929-4c55-942b-97f5099a728a" />


результат программы:


<img width="240" height="229" alt="Снимок экрана 2026-05-20 175415" src="https://github.com/user-attachments/assets/9a4d2748-0cd9-4301-b568-115b63357cba" />


сгенерированный ассемблерный код без оптимизации:

```
.file	"fact.cpp"
.text
.globl	_Z9factoriali
.def	_Z9factoriali;	.scl	2;	.type	32;	.endef
.seh_proc	_Z9factoriali
_Z9factoriali:
.LFB2623:
	pushq	%rbp                     ; сохраняем старый base pointer
.seh_pushreg	%rbp
	movq	%rsp, %rbp               ; устанавливаем новый base pointer
.seh_setframe	%rbp, 0
	subq	$16, %rsp                ; выделяем 16 байт на стеке под локальные переменные
.seh_stackalloc	16
.seh_endprologue
	movl	%ecx, 16(%rbp)           ; ПЕРЕМЕННАЯ n = аргумент (сохраняем на стек)
	movq	$1, -8(%rbp)             ; ПЕРЕМЕННАЯ result = 1
	movl	$2, -12(%rbp)            ; ПЕРЕМЕННАЯ i = 2 (счётчик цикла)
	jmp	.L2                      ; переход к проверке условия

.L3:                             ; НАЧАЛО ТЕЛА ЦИКЛА
	movl	-12(%rbp), %eax          ; загружаем i в eax
	cltq                             ; расширяем i до 64 бит (в rax)
	movq	-8(%rbp), %rdx           ; загружаем result в rdx
	imulq	%rdx, %rax               ; УМНОЖЕНИЕ: rax = i * result
	movq	%rax, -8(%rbp)           ; сохраняем result обратно на стек
	addl	$1, -12(%rbp)            ; ИНКРЕМЕНТ: i++

.L2:                             ; ПРОВЕРКА УСЛОВИЯ ЦИКЛА
	movl	-12(%rbp), %eax          ; загружаем i
	cmpl	16(%rbp), %eax           ; сравниваем i и n
	jle	.L3                      ; ЕСЛИ i <= n ТО переход на L3 (продолжаем цикл)

	movq	-8(%rbp), %rax           ; загружаем result в rax для возврата
	addq	$16, %rsp                ; освобождаем стек
	popq	%rbp                     ; восстанавливаем base pointer
	ret                              ; возврат из функции
.seh_endproc

.section .rdata,"dr"
.LC0:
	.ascii "\302\342\345\344\350\362\345 \367\350\361\353\356: \0"   ; СТРОКА "Введите число: "
.LC1:
	.ascii "! = \0"                   ; СТРОКА "! = "
.text
.globl	main
.def	main;	.scl	2;	.type	32;	.endef
.seh_proc	main
main:
.LFB2624:
	pushq	%rbp                     ; сохраняем rbp
.seh_pushreg	%rbp
	pushq	%rbx                     ; сохраняем rbx
.seh_pushreg	%rbx
	subq	$56, %rsp                ; выделяем 56 байт на стеке
.seh_stackalloc	56
	leaq	48(%rsp), %rbp           ; настраиваем base pointer
.seh_setframe	%rbp, 48
.seh_endprologue
	call	__main                   ; инициализация C++ (конструкторы глобальных объектов)

	; ВЫВОД СТРОКИ "Введите число: "
	leaq	.LC0(%rip), %rdx         ; rdx = указатель на строку
	movq	.refptr._ZSt4cout(%rip), %rax
	movq	%rax, %rcx               ; rcx = cout
	call	_ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc  ; cout << "..."

	; ВВОД ЧИСЛА cin >> n
	leaq	-4(%rbp), %rdx           ; rdx = адрес ПЕРЕМЕННОЙ n на стеке
	movq	.refptr._ZSt3cin(%rip), %rax
	movq	%rax, %rcx               ; rcx = cin
	call	_ZNSirsERi               ; cin >> n

	; ВЫВОД ЧИСЛА cout << n
	movl	-4(%rbp), %edx           ; edx = n (значение из стека)
	movq	.refptr._ZSt4cout(%rip), %rax
	movq	%rax, %rcx               ; rcx = cout
	call	_ZNSolsEi                ; cout << n

	; ВЫВОД СТРОКИ "! = "
	movq	%rax, %rcx               ; сохраняем cout
	leaq	.LC1(%rip), %rax         ; rax = указатель на "! = "
	movq	%rax, %rdx               ; rdx = строка
	call	_ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc  ; cout << "! = "

	; ВЫЗОВ ФУНКЦИИ factorial(n)
	movq	%rax, %rbx               ; сохраняем cout в rbx
	movl	-4(%rbp), %eax           ; eax = n
	movl	%eax, %ecx               ; ecx = n (аргумент для функции)
	call	_Z9factoriali            ; ВЫЗОВ ФУНКЦИИ factorial(n), результат в rax

	; ВЫВОД РЕЗУЛЬТАТА cout << результат
	movq	%rax, %rdx               ; rdx = результат factorial
	movq	%rbx, %rcx               ; rcx = cout
	call	_ZNSolsEx                ; cout << результат (long long)

	; ВЫВОД ПЕРЕВОДА СТРОКИ cout << endl
	movq	%rax, %rcx               ; rcx = cout
	movq	.refptr._ZSt4endlIcSt11char_traitsIcEERSt13basic_ostreamIT_T0_ES6_(%rip), %rax
	movq	%rax, %rdx               ; rdx = endl
	call	_ZNSolsEPFRSoS_E         ; cout << endl

	movl	$0, %eax                 ; return 0
	addq	$56, %rsp                ; освобождаем стек
	popq	%rbx                     ; восстанавливаем rbx
	popq	%rbp                     ; восстанавливаем rbp
	ret                              ; возврат из main
.seh_endproc

.def	__main;	.scl	2;	.type	32;	.endef
.ident	"GCC: (Rev5, Built by MSYS2 project) 16.1.0"
.def	_ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc;	.scl	2;	.type	32;	.endef
.def	_ZNSirsERi;	.scl	2;	.type	32;	.endef
.def	_ZNSolsEi;	.scl	2;	.type	32;	.endef
.def	_ZNSolsEx;	.scl	2;	.type	32;	.endef
.def	_ZNSolsEPFRSoS_E;	.scl	2;	.type	32;	.endef
.section	.rdata$.refptr._ZSt4endlIcSt11char_traitsIcEERSt13basic_ostreamIT_T0_ES6_, "dr"
.p2align	3, 0
.globl	.refptr._ZSt4endlIcSt11char_traitsIcEERSt13basic_ostreamIT_T0_ES6_
.linkonce	discard
.refptr._ZSt4endlIcSt11char_traitsIcEERSt13basic_ostreamIT_T0_ES6_:
	.quad	_ZSt4endlIcSt11char_traitsIcEERSt13basic_ostreamIT_T0_ES6_
.section	.rdata$.refptr._ZSt3cin, "dr"
.p2align	3, 0
.globl	.refptr._ZSt3cin
.linkonce	discard
.refptr._ZSt3cin:
	.quad	_ZSt3cin
.section	.rdata$.refptr._ZSt4cout, "dr"
.p2align	3, 0
.globl	.refptr._ZSt4cout
.linkonce	discard
.refptr._ZSt4cout:
	.quad	_ZSt4cout
```



С оптимизацией (-O3):


```
	.file	"fact.cpp"
	.text
	.section	.text$_ZNKSt5ctypeIcE8do_widenEc,"x"
	.linkonce discard
	.align 2
	.p2align 4
	.globl	_ZNKSt5ctypeIcE8do_widenEc
	.def	_ZNKSt5ctypeIcE8do_widenEc;	.scl	2;	.type	32;	.endef
	.seh_proc	_ZNKSt5ctypeIcE8do_widenEc
_ZNKSt5ctypeIcE8do_widenEc:
.LFB2410:
	.seh_endprologue
	movl	%edx, %eax
	ret
	.seh_endproc
	.text
	.p2align 4
	.globl	_Z9factoriali
	.def	_Z9factoriali;	.scl	2;	.type	32;	.endef
	.seh_proc	_Z9factoriali
_Z9factoriali:
.LFB2666:
	.seh_endprologue
	cmpl	$1, %ecx
	jle	.L6
	leal	1(%rcx), %r8d
	movl	$2, %eax
	movl	$1, %edx
	testb	$1, %r8b
	je	.L5
	movl	$3, %eax
	movl	$2, %edx
	cmpq	%r8, %rax
	je	.L3
	.p2align 5
	.p2align 4
	.p2align 3
.L5:
	imulq	%rax, %rdx
	leaq	1(%rax), %rcx
	addq	$2, %rax
	imulq	%rcx, %rdx
	cmpq	%r8, %rax
	jne	.L5
.L3:
	movq	%rdx, %rax
	ret
	.p2align 4,,10
	.p2align 3
.L6:
	movl	$1, %edx
	movq	%rdx, %rax
	ret
	.seh_endproc
	.section .rdata,"dr"
.LC0:
	.ascii "\302\342\345\344\350\362\345 \367\350\361\353\356: \0"
.LC1:
	.ascii "! = \0"
	.section	.text.startup,"x"
	.p2align 4
	.globl	main
	.def	main;	.scl	2;	.type	32;	.endef
	.seh_proc	main
main:
.LFB2667:
	pushq	%rbx
	.seh_pushreg	%rbx
	subq	$64, %rsp
	.seh_stackalloc	64
	.seh_endprologue
	call	__main
	movq	.refptr._ZSt4cout(%rip), %rbx
	movl	$15, %r8d
	leaq	.LC0(%rip), %rdx
	movq	%rbx, %rcx
	call	_ZSt16__ostream_insertIcSt11char_traitsIcEERSt13basic_ostreamIT_T0_ES6_PKS3_x
	movq	.refptr._ZSt3cin(%rip), %rcx
	leaq	60(%rsp), %rdx
	call	_ZNSirsERi
	movl	60(%rsp), %edx
	movq	%rbx, %rcx
	call	_ZNSolsEi
	movl	$4, %r8d
	leaq	.LC1(%rip), %rdx
	movq	%rax, %rcx
	movq	%rax, %rbx
	call	_ZSt16__ostream_insertIcSt11char_traitsIcEERSt13basic_ostreamIT_T0_ES6_PKS3_x
	movl	60(%rsp), %eax
	cmpl	$1, %eax
	jle	.L19
	leal	1(%rax), %r8d
	movl	$1, %edx
	movl	$2, %eax
	testb	$1, %r8b
	je	.L15
	movl	$3, %eax
	movl	$2, %edx
	cmpq	%r8, %rax
	je	.L14
	.p2align 5
	.p2align 4
	.p2align 3
.L15:
	imulq	%rax, %rdx
	leaq	1(%rax), %rcx
	addq	$2, %rax
	imulq	%rcx, %rdx
	cmpq	%r8, %rax
	jne	.L15
.L14:
	movq	%rbx, %rcx
	call	_ZNSo9_M_insertIxEERSoT_
	movq	%rax, %r8
	movq	(%rax), %rax
	movq	-24(%rax), %rax
	movq	240(%r8,%rax), %rbx
	testq	%rbx, %rbx
	je	.L27
	cmpb	$0, 56(%rbx)
	je	.L17
	movsbl	67(%rbx), %edx
.L18:
	movq	%r8, %rcx
	call	_ZNSo3putEc
	movq	%rax, %rcx
	call	_ZNSo5flushEv
	xorl	%eax, %eax
	addq	$64, %rsp
	popq	%rbx
	ret
.L17:
	movq	%rbx, %rcx
	movq	%r8, 40(%rsp)
	call	_ZNKSt5ctypeIcE13_M_widen_initEv
	movq	(%rbx), %rax
	movq	40(%rsp), %r8
	leaq	_ZNKSt5ctypeIcE8do_widenEc(%rip), %rcx
	movl	$10, %edx
	movq	48(%rax), %rax
	cmpq	%rcx, %rax
	je	.L18
	movl	$10, %edx
	movq	%rbx, %rcx
	call	*%rax
	movq	40(%rsp), %r8
	movsbl	%al, %edx
	jmp	.L18
.L19:
	movl	$1, %edx
	jmp	.L14
.L27:
	call	_ZSt16__throw_bad_castv
	nop
	.seh_endproc
	.def	__main;	.scl	2;	.type	32;	.endef
	.ident	"GCC: (Rev5, Built by MSYS2 project) 16.1.0"
	.def	_ZSt16__ostream_insertIcSt11char_traitsIcEERSt13basic_ostreamIT_T0_ES6_PKS3_x;	.scl	2;	.type	32;	.endef
	.def	_ZNSirsERi;	.scl	2;	.type	32;	.endef
	.def	_ZNSolsEi;	.scl	2;	.type	32;	.endef
	.def	_ZNSo9_M_insertIxEERSoT_;	.scl	2;	.type	32;	.endef
	.def	_ZNSo3putEc;	.scl	2;	.type	32;	.endef<img width="691" height="284" alt="Снимок экрана 2026-05-20 190219" src="https://github.com/user-attachments/assets/9f148fe5-7fc0-4d76-b758-980878e3b8c0" />
	.def	_ZNKSt5ctypeIcE13_M_widen_initEv;	.scl	2;	.type	32;	.endef
	.def	_ZSt16__throw_bad_castv;	.scl	2;	.type	32;	.endef
	.section	.rdata$.refptr._ZSt3cin, "dr"
	.p2align	3, 0
	.globl	.refptr._ZSt3cin
	.linkonce	discard
.refptr._ZSt3cin:
	.quad	_ZSt3cin
	.section	.rdata$.refptr._ZSt4cout, "dr"
	.p2align	3, 0
	.globl	.refptr._ZSt4cout
	.linkonce	discard
.refptr._ZSt4cout:
	.quad	_ZSt4cout
```
Makefile:


<img width="345" height="313" alt="Снимок экрана 2026-05-20 190236" src="https://github.com/user-attachments/assets/2e18c7f8-a046-43db-b47e-d5474d2e69fa" />



результат makefile:


<img width="691" height="284" alt="Снимок экрана 2026-05-20 190219" src="https://github.com/user-attachments/assets/972ba911-b0d5-478f-8b49-c9f7102e27da" />


Добавление параллельного потока и синхронизация:


<img width="670" height="765" alt="Снимок экрана 2026-05-20 190710" src="https://github.com/user-attachments/assets/49b4685f-4633-492a-a432-95f661899254" />


результат:


<img width="528" height="193" alt="Снимок экрана 2026-05-20 190654" src="https://github.com/user-attachments/assets/b5c74975-307b-4032-b2a1-14aaffd401d5" />


Обновляем Makefile:


<img width="501" height="618" alt="image" src="https://github.com/user-attachments/assets/54e9a5e3-5842-407a-9e3b-5d53fffbf580" />


Результат:


<img width="771" height="388" alt="image" src="https://github.com/user-attachments/assets/34aa30a6-ff8d-43d7-9505-edcf4cc1b303" />


Задание № 2


[https://cloud.mail.ru/home/](https://cloud.mail.ru/public/AqQj/CuBTUWx8W)


(последние минуты не записались, прошу прощения)










