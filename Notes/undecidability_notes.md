<h1>Undecidability Notes</h1>

Recursively enumerable lanuages - are lanuaguages that are simply acceptable. That is for a given language and its corresponding TM M, $Accept(M) = L$ (the set of all stirngs that it accepts is equal to L). However, this does not imply that it rejects all strings not in L

Recursive Languages is the set of all languages that are decidable. More specifically, all language corresponging to a TM M that halts on all inputs in a given alphabet for a given alphabet and $Accept(M) = L$ and $Diverge(M) = \empty$

A Language L is Undecidable iff it is not decidable.

A lanugage L is **decidable** iff:

There exists a Turing Machine M such that $Accept(M) = L$ and $Diverge(M) = \empty$, which implies that $Reject(M) = \Sigma^* - L$ and $HALT(M) = \Sigma^*$

This means that there has to be a turing Machine M that can satisfy these properties

The Universal Lanugage:
$M_u = \{\langle M, w\rangle \ | \ M \ \text{accepts} \ w \}$

This is the set of all string encodings of a Turing Machine M and a string w, such that that specific machine accepts w.

## Self Reject

Consider the language SelfReject:

$$\text{SelfReject} = \{ \ \langle M \rangle \ | \ M \ \text{rejects} \ \langle M \rangle \}$$

The important obervation is to note that $\langle M \rangle$ is an encoded string (finite legnth binary string encoding the information of a specific TM).

**Proof:** SelfReject is undecidable

Suppose not, that is suppose that there exists a TM $SR$ that decides SelfReject. This then implies that $Accept(SR) = \text{SelfReject}$ and $Diverge(SR) = \empty$.

More explicily, for any turing machine M:

- $SR$ accepts $\langle M \rangle \iff M$ rejects $\langle M \rangle $
- $SR$ rejects $\langle M \rangle \iff M$ does not reject $\langle M \rangle $

We say does not reject because if it diverges on its own encoding, then that still does not count.

In particular, these equivalences must hold for the machine encoding itself $\langle SR \rangle$ becasue it is also a TM, at it is either accepted or rejected by some machine. Thus,

- $SR$ accepts $\langle SR \rangle \iff SR$ rejects $\langle SR \rangle $
- $SR$ rejects $\langle SR \rangle \iff M$ does not reject $\langle SR \rangle $

However, SR can only accept its own encoding if it can reject its own encoding, which is a contradiction. Thus implying that such a machine cannot exist.

## The Halting Problem

Consider the language $HALT$:
$$HALT =\{\ \langle M, w \rangle \ | \ M \ \text{halts on} \ w \}$$

The halting problem is given a program M and a input w, does the program halt. It is easy to accept HALT, that just means that everyhting in H is accepted by our machine, which is trivial beacause the program will accept those inputs. However, we want to actually decide Halt, which means that not only must Accept(H) = HALT, but Diverge(H) = the empty set. The latter being impossible to prove. Hence we can use a reduction from a known problem, SelfHalt to Halt, which uses it as a subroutine.

Suppose not, that is suppose there is a Turing Machine H that decides HALT. We can use H to build another Turing Machine SH that decides the language SelfHalt. Given a string w and and M, the machine first verifies that $w = \langle M \rangle $, it must halt on its own input, and rejects otherwise. Then it writes the string $ww = \langle M, M\rangle $ onto the tape, and finaly passes control to H. Further, we pass $ww = \langle M, M\rangle $ to H, because H will accept it if an only if it halts on the input, and self halt would be false on that input. However, because SH does not exist, that would imply that H cannot exists.

$\langle M, w \rangle $ -> SH -> Reduce($\langle M, w \rangle$) = $\langle M, M \rangle$ -> H

- $SH$ accepts $\langle M, w \rangle \iff H$ does not halt on $\langle M, M\rangle $

Formulate this type of reduction
