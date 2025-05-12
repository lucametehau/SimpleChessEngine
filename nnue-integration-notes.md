Model NNUE Default Settings 

374281 nodes 3898760 nps


Elo   | 69.64 +- 13.44 (95%)
SPRT  | 8.0+0.08s Threads=1 Hash=8MB
LLR   | 2.95 (-2.94, 2.94) [0.00, 3.00]
Games | N: 1264 W: 470 L: 220 D: 574
Penta | [10, 97, 237, 209, 79]


mod activation;
mod inputs;
mod network;
mod outputs;
mod trainer;

use trainer::{lr, wdl, DirectSequentialDataLoader, LocalSettings, Trainer, TrainingSchedule, TrainingSteps};

// Network architecture settings
pub type InputFeatures = inputs::Chess768;
pub type OutputBuckets = outputs::Single;
pub type Activation = activation::SCReLU;
pub const HL_SIZE: usize = 128;

// Quantisations
pub const QA: i16 = 255;
pub const QB: i16 = 64;

/// Applicable only with `InputFeatures` option `ChessBucketsMirrored`.
/// Indexed from white POV, so index 0 corresponds to A1, 3 corresponds to D1.
#[rustfmt::skip]
pub const BUCKETS_MIRRORED: [usize; 32] = [
    0, 0, 0, 0,
    0, 0, 0, 0,
    1, 1, 1, 1,
    1, 1, 1, 1,
    1, 1, 1, 1,
    1, 1, 1, 1,
    1, 1, 1, 1,
    1, 1, 1, 1,
];

fn main() {
    let mut trainer = Trainer::default();
    // let mut trainer = Trainer::from_checkpoint("checkpoints/legacy-10").unwrap();

    let loader = DirectSequentialDataLoader::new(&["data/baseline.data"]);

    let schedule = TrainingSchedule {
        net_id: "legacy".to_string(),
        eval_scale: 400.0,
        steps: TrainingSteps {
            batch_size: 16_384,
            batches_per_superbatch: 6104,
            start_superbatch: 1,
            end_superbatch: 40,
        },
        wdl_scheduler: wdl::ConstantWDL { value: 0.75 },
        lr_scheduler: lr::StepLR { start: 0.001, gamma: 0.1, step: 25 },
        save_rate: 10,
    };

    let settings = LocalSettings { threads: 6, output_directory: "checkpoints", batch_queue_size: 64 };

    trainer.run(loader, &schedule, &settings);
}
