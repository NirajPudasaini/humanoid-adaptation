# humanoid-adaptation

Reading notes on online adaptation for legged and humanoid control.

Work in this area gets lumped together as "adaptation," which hides the distinction that
actually drives the architecture: what is unknown, how the policy gets hold of it, and how
fast it changes. A method that regresses a hidden terrain latent from proprioceptive history
and a method that reads an external force off an inverse-dynamics estimate are solving
different problems, even when both are called online adaptation.

So this repo sorts papers by how a policy obtains its context, not by application domain.
A quadruped terrain paper and a humanoid force paper end up next to each other when the
mechanism is the same. Each paper is one file under `papers/` with its citation and a short
description.

## Sections

Five of them, ordered roughly by how directly the policy is given what it needs.

**[`01-meta-rl`](papers/01-meta-rl)** Where adaptation was first posed as a learning problem.
Both branches: gradient-based, where a test-time update changes the weights, and memory-based,
where a recurrent or attentional policy adapts inside its hidden state with no gradient at all.

**[`02-teacher-student`](papers/02-teacher-student)** Train a teacher with privileged access to
the unknown, then train a student to recover the same information from what the robot can
actually sense. Two stages, and a student that can never beat its regression target.

**[`03-implicit-estimation`](papers/03-implicit-estimation)** Same goal, one stage. The latent is
learned jointly with the policy through the critic, a VAE, or a self-supervised prediction loss.
Nothing is named, so nothing has to be nameable.

**[`04-in-context`](papers/04-in-context)** No estimation module and no test-time gradient. A
transformer over a long history of what the robot did and what happened next.

**[`05-explicit-conditioning`](papers/05-explicit-conditioning)** Hand the policy the varying
quantity. No inference problem, so no history requirement. The open questions are whether the
quantity can be obtained at all, and where in the network it should enter.

## Adding a paper

Copy `templates/paper-note.md` into the section that matches how the method obtains its context,
and name it after the citekey (`kumar2021rma.md`).
