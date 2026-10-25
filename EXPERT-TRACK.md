# Expert Track: Scale, Real Deployments, and Proof

A syllabus addition to the AI Engineering from Scratch course (added 2026-10-25).

The core curriculum teaches every mechanism from first principles at laptop
scale. This track adds what it leaves out. Experts have run systems at real
scale, deployed them for real users, operated them through failures, and
can show the evidence. Each item below hooks into a phase and is part of
that phase's gate, so a phase is finished only when its expert work is done.

## Ground rules

- **Budget first.** Before any cloud run, write down the expected cost, set
  a budget alert, and turn on auto-shutdown for GPU machines. Record what
  each run actually cost. Being able to say "this cost $X and here is why"
  is part of the skill.
- **Use your own subscription for this work.** Use a personal Azure
  subscription (or a GPU rental service) unless your employer has confirmed
  in writing that its subscription may be used for it.
- **Use public data or data you are authorized to use.** Never use company
  or client data.
- **Evidence or it didn't happen.** Every expert lab produces a short
  write-up in `learning/expert/<NN-slug>/REPORT.md` with: the goal, the
  setup, the numbers (throughput, latency, cost, quality), what broke, and
  what you would tell a client.

## Part 1: Scale labs

| After phase | Lab | What you prove |
|---|---|---|
| 3 Deep Learning | **First GPU run.** Train your own from-scratch network on a rented single GPU, then compare it with the same run on your laptop. Profile it with the PyTorch profiler. | You can measure speed and cost and explain where the time goes. |
| 5 NLP | **Data at scale.** Use a 10–100 GB public text corpus. Stream it, deduplicate it, filter it for quality, and tokenize it into shards (preview of capstones 42–43). | Real data is messy. You can build a pipeline that doesn't run out of memory. |
| 10 LLMs from Scratch | **Pre-train a GPT-2-class model (about 124M parameters)** on real tokens, first on one GPU, then on several GPUs with DDP and FSDP. Track loss, tokens per second, the share of the GPU's peak speed you used (MFU), and cost. | The mechanisms you simulated on your laptop (lesson 05, capstones 76–81) hold up for real. |
| 10 LLMs from Scratch | **Fine-tune an open model** with LoRA, SFT and DPO on a public dataset. Evaluate it against the base model and against a model reached through an API. | You can tell when fine-tuning beats prompting, with numbers. |
| 11 LLM Engineering | **RAG at scale.** Index at least 1M documents. Measure retrieval recall at several result counts, latency at p50 and p95, index cost, and how fresh the data stays. Compare dense, hybrid and reranked retrieval. | Retrieval quality and cost at the size clients actually have. |
| 11 LLM Engineering | **Evaluation at scale.** Build a set of more than 1,000 test cases. Check the LLM judge's agreement with your own labels. Report confidence intervals from bootstrap resampling, and gate releases on regressions. | You can tell whether a change actually improved results or was just noise. |
| 17 Infrastructure | **Self-hosted inference.** Serve an open model with vLLM on a cloud GPU. Load-test it, measuring time to first token, tokens per second, and throughput at different concurrency levels. Try quantization. Compare cost per 1M tokens with your Azure API deployments. | The build-vs-buy table every client asks for, with your own measurements. |
| 17 Infrastructure | **Systems depth (optional, strongly differentiating).** Write one GPU kernel in Triton, for example a fused softmax or a simple flash-attention, and benchmark it. | You understand what actually limits GPU speed. |

## Part 2: The deployment ladder

Every major system you build climbs the same ladder. Starting after Phase 11,
the RAG system, the agent (Phase 14) and the capstone flagship (Phase 19)
must each reach rung 8.

1. **Local:** runs and passes its tests on your laptop.
2. **Containerized:** a Docker image, configured entirely through environment
   variables.
3. **Deployed with infrastructure as code:** on Azure (Container Apps, or AKS
   for Kubernetes work) using Bicep or Terraform. No manual changes in the
   portal.
4. **Automated release pipeline (CI/CD):** GitHub Actions runs tests and
   evals, builds the container, and deploys. A failing eval blocks the
   release.
5. **Secured:** keyless sign-in with managed identity, secrets in Key Vault,
   least-privilege access, content safety, and a prompt-injection red-team
   pass (Phase 18 lessons 12–16).
6. **Observable:** OpenTelemetry traces for every model and tool call, cost
   per request, dashboards, and alerts (Phase 17 lessons 13 and 23).
7. **Proven under load and failure:** a load test (lesson 22), a chaos drill
   (lesson 24: kill a dependency, hit a rate limit, simulate a model outage
   and check the fallback), and a written postmortem.
8. **Operated:** live for at least 30 days, with targets for uptime and
   latency, a weekly eval run, drift checks, a runbook, and a monthly
   operations report.

## Part 3: Real users

Scale and infrastructure don't count as experience until real users depend
on what you built.

- **Two public deployments.** Ship two of your systems as public tools or
  open-source projects that people other than you actually use. Measure
  usage and feedback.
- **One pro-bono pilot.** Run the forward-deployed path (Phase 14 lessons
  47–54, Phase 17 lesson 20) with a real organization, for example a
  nonprofit or a small business. Cover discovery, scoping, a measured pilot,
  gradual rollout, and a written case study with their permission. This is
  the strongest proof a consultant can show.

## Part 4: What sets an expert consultant apart

- **One paper per phase.** Read the original paper behind the phase's key
  lesson. Once per term, reproduce one of its results.
- **Governance and regulation.** For each deployed system, produce a risk
  assessment mapped to the NIST AI RMF and the EU AI Act, plus model and
  system cards (Phase 18 lessons 24 and 26, Phase 17 lesson 26).
- **Decision memos.** At each phase end, write a one-page client memo for a
  real decision. Examples: build vs. buy, RAG vs. fine-tuning, which cloud
  and which model, agent vs. workflow. Each memo cites your own measurements.
- **Unit economics.** Keep a cost model covering cost per request and per
  task, savings from caching, and savings from routing between models
  (Phase 17 lessons 02 and 27). Clients buy outcomes at a price.
- **Specialization (decided at the end of Phase 14).** Pick one or two deep
  areas, such as evaluation and reliability, enterprise agents, or inference
  optimization, and go past the curriculum: papers, open-source
  contributions, and harder labs.
- **Public proof.** Publish a write-up for each phase, built from your reports
  and playbook. Contribute fixes to open-source AI projects, including this
  curriculum under its contribution rules. Clients find consultants through
  work they can see.

## Gate summary

A phase is finished when all four of these are done:
1. its lessons are passed and its review queue is clear;
2. the from-memory exam is passed;
3. its expert-track items from the tables above are completed, with a
   REPORT.md for each;
4. its decision memo is written.
