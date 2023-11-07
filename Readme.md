# 11 Nov 2023 13:00+03:00UTC
## prover sync
## named constraints sync
...

# 07 Nov 2023

---

## protostar prover: (aliventer)

given two relaxed instances A, B (relaxed here just means a single additional error value for major gate)
compute majgate(A+t(B-A))

compute each constraint on A+t(B-A)

step 1: evaluate each constr of deg d on t=0, t=1, ..., t=d
// probably makes sense to find for each witness element the max degree of a constraint it participates in

step 2: use my magic function to obtain values of the majgate in t=0 ... t = D+log_ceil(m)

?????

step 3: folding

## batch invert; advice propagation.
**postponed**

```
circuit.add_chest::<BatchInversion>({})

circuit.get_chests::<BatchInversion>()

trait Finalizer{
    fn finalize_construction(&mut circuit);
    fn finalize_execution(&mut circuit);
}

struct BatchInversion {
    a: Variable
    b: Variable
}

impl Finalizer for BatchInversion {
    fn finalize_construction(&mut circuit) {
        circuit.get_chests(Self);
    }
    fn finalize_execution(&mut circuit) {
        circuit.get_chests(Self);
    }
}


End of construction phase {
    ...
    BatchInversion::finalize_construction()
    ...
}

End of execution phase {
    ...
    BatchInversion::finalize_execution()
    ...
}

```

## Named constraints: (rebenkoy)

- separate library for constraints
- gadget declares used constraints
- user specifies used constraints in circuit at startup.


## Varlike (Burn it with fire)

### idea for future: 
Varlike "constraint" explorer. Probably burn it with fire.

### discussuion
---

pub trait SignalTrait {}

impl SignalTrait for Variable {}
impl<F: Field> SignalTrait for F {}

---

Possibly needed structures: 
- Varlike
- IntermediateRepr 💩💩💩💩💩💩💩💩💩💩💩💩💩 (will not be done in this iteration)
- Fieldlike

pub struct SmallVar : Varlike {
    value: Variable,
    range: u64
}

pub struct SmallVarIR : IntermediateRepr {
    value: u64,
    range: u64,
} 💩💩💩💩💩💩💩💩💩💩💩💩💩💩💩💩💩

pub struct SmallVarF : Fieldlike {
    value: F,
    range: u64,
}

---

struct Foo<T: SignalTrait>{
    a: T,
    b: T,
}

impl<F: Field> Varlike<F> for Foo<Variable> { 
    type TargetFlike = Foo<F>;
}

---

макрос macro! это "sed 's/Variable/Field/g'"


macro!(
struct SmallVar {
    a: Variable,
    b: uszie,  // this thing has no idea how to propagate further if it gets stripped of.
}

impl MagicallySerializable<Variable> for SmallVar { 
    fn serialize(self) -> Vec<Variable> {
        
    }
}
)

---


impl Magic for (A: Magic, B:Magic) {
    
}

type MyStructV = MyStruct<Variable>;
type MyStructF<F> = MyStruct<F>;

impl<F: Field> Varlike for MyStructV {
    type TargetF = MyStructF<F>;
}

Burn it with fire!

