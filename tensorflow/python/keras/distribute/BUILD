# Description:
#   keras/distribute package is intended to serve as the centralized place for things
#   related to dist-strat used by Keras..

load("//tensorflow/core/platform/default:distribute.bzl", "distribute_py_test")
load("//tensorflow:tensorflow.bzl", "py_test")
load("//tensorflow:tensorflow.bzl", "cuda_py_test")

package(
    # TODO(scottzhu): Remove this deps when distribute test are converted to integration test.
    default_visibility = [
        "//tensorflow/python/distribute:__pkg__",
        "//tensorflow/python/keras:__subpackages__",
        "//tensorflow/tools/pip_package:__pkg__",
    ],
    licenses = ["notice"],  # Apache 2.0
)

filegroup(
    name = "all_py_srcs",
    srcs = glob(["*.py"]),
    visibility = ["//tensorflow/python/keras/google/private_tf_api_test:__pkg__"],
)

py_library(
    name = "distribute",
    srcs = [
        "__init__.py",
        "distributed_training_utils.py",
        "distributed_training_utils_v1.py",
    ],
    srcs_version = "PY2AND3",
    deps = [
        "//tensorflow/python:client_testlib",
        "//tensorflow/python/data",
        "//tensorflow/python/distribute:distribute_coordinator",
        "//tensorflow/python/distribute:distribute_lib",
        "//tensorflow/python/distribute:input_lib",
        "//tensorflow/python/distribute:one_device_strategy",
        "//tensorflow/python/distribute:reduce_util",
        "//tensorflow/python/distribute:values",
        "//tensorflow/python/eager:def_function",
        "//tensorflow/python/keras:activations",
        "//tensorflow/python/keras:backend",
        "//tensorflow/python/keras:callbacks",
        "//tensorflow/python/keras:callbacks_v1",
        "//tensorflow/python/keras:constraints",
        "//tensorflow/python/keras:initializers",
        "//tensorflow/python/keras:losses",
        "//tensorflow/python/keras:optimizers",
        "//tensorflow/python/keras:regularizers",
        "//tensorflow/python/keras/mixed_precision/experimental:autocast_variable",
        "//tensorflow/python/keras/mixed_precision/experimental:policy",
        "//tensorflow/python/keras/saving",
        "//tensorflow/python/keras/utils:engine_utils",
        "//tensorflow/python/keras/utils:mode_keys",
        "//tensorflow/python/training/tracking:data_structures",
        "//tensorflow/tools/docs:doc_controls",
    ],
)

py_library(
    name = "distribute_test_lib_pip",
    deps = [
        ":distribute_strategy_test_lib",
        ":keras_correctness_test_lib",
        ":keras_test_lib",
        ":model_combinations",
        ":multi_worker_testing_utils",
        ":saved_model_test_base",
    ],
)

py_library(
    name = "optimizer_combinations",
    srcs = ["optimizer_combinations.py"],
    srcs_version = "PY2AND3",
    deps = [
        "//tensorflow/python:training",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/keras/optimizer_v2",
    ],
)

py_library(
    name = "worker_training_state",
    srcs = [
        "worker_training_state.py",
    ],
    srcs_version = "PY2AND3",
)

py_library(
    name = "model_collection_base",
    srcs = ["model_collection_base.py"],
)

py_library(
    name = "model_combinations",
    srcs = ["model_combinations.py"],
    srcs_version = "PY2AND3",
    deps = [
        ":simple_models",
        "//tensorflow/python/distribute:combinations",
    ],
)

py_library(
    name = "simple_models",
    srcs = ["simple_models.py"],
    deps = [
        ":model_collection_base",
        "//tensorflow/python:constant_op",
        "//tensorflow/python:dtypes",
        "//tensorflow/python/keras",
    ],
)

py_library(
    name = "saved_model_test_base",
    srcs = ["saved_model_test_base.py"],
    deps = [
        ":model_combinations",
        "//tensorflow/python:array_ops",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/saved_model",
        "//third_party/py/numpy",
    ],
)

cuda_py_test(
    name = "worker_training_state_test",
    srcs = ["worker_training_state_test.py"],
    python_version = "PY3",
    shard_count = 4,
    tfrt_enabled = True,
    deps = [
        ":multi_worker_testing_utils",
        ":worker_training_state",
        "//tensorflow/python:platform",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:multi_worker_test_base",
        "//tensorflow/python/keras",
    ],
)

distribute_py_test(
    name = "checkpointing_test",
    srcs = ["checkpointing_test.py"],
    main = "checkpointing_test.py",
    tags = [
        "multi_and_single_gpu",
    ],
    deps = [
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/keras/optimizer_v2",
        "//tensorflow/python/training/tracking:util",
    ],
)

cuda_py_test(
    name = "collective_all_reduce_strategy_test",
    srcs = ["collective_all_reduce_strategy_test.py"],
    python_version = "PY3",
    tags = [
        "multi_and_single_gpu",
        "nomsan",  # TODO(b/162894966)
    ],
    # b/155301154 broken with XLA:GPU
    xla_enable_strict_auto_jit = True,
    deps = [
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:dtypes",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:framework_test_lib",
        "//tensorflow/python:nn",
        "//tensorflow/python:random_ops",
        "//tensorflow/python:training_lib",
        "//tensorflow/python:training_util",
        "//tensorflow/python:variables",
        "//tensorflow/python/compat:v2_compat",
        "//tensorflow/python/distribute:collective_all_reduce_strategy",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:cross_device_utils",
        "//tensorflow/python/distribute:multi_worker_test_base",
        "//tensorflow/python/distribute:multi_worker_util",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/distribute:strategy_test_lib",
        "//tensorflow/python/distribute/cluster_resolver:cluster_resolver_lib",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/keras:testing_utils",
        "//tensorflow/python/keras/engine",
        "//tensorflow/python/keras/layers",
        "//tensorflow/python/keras/mixed_precision/experimental:policy",
        "//tensorflow/python/keras/mixed_precision/experimental:test_util",
        "//tensorflow/python/ops/losses",
        "@absl_py//absl/testing:parameterized",
    ],
)

distribute_py_test(
    name = "ctl_correctness_test",
    srcs = ["ctl_correctness_test.py"],
    main = "ctl_correctness_test.py",
    shard_count = 10,
    tags = [
        "multi_and_single_gpu",
    ],
    deps = [
        ":optimizer_combinations",
        "//tensorflow/python:platform_test",
        "//tensorflow/python:util",
        "//tensorflow/python/compat:v2_compat",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/keras",
    ],
)

distribute_py_test(
    name = "custom_training_loop_metrics_test",
    srcs = ["custom_training_loop_metrics_test.py"],
    disable_mlir_bridge = False,
    main = "custom_training_loop_metrics_test.py",
    tags = [
        "multi_and_single_gpu",
    ],
    deps = [
        "//tensorflow/python:errors",
        "//tensorflow/python:variables",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/keras:metrics",
        "@absl_py//absl/testing:parameterized",
    ],
)

distribute_py_test(
    name = "custom_training_loop_models_test",
    srcs = ["custom_training_loop_models_test.py"],
    main = "custom_training_loop_models_test.py",
    tags = [
        "multi_and_single_gpu",
        "notsan",  # TODO(b/170954243)
    ],
    tpu_tags = [
        "no_oss",  # b/153615544.
        "notsan",  # TODO(b/170869466)
    ],
    deps = [
        "//tensorflow/python:math_ops",
        "//tensorflow/python:util",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:reduce_util",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/eager:backprop",
        "//tensorflow/python/eager:def_function",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/keras",
        "//tensorflow/python/module",
        "@absl_py//absl/testing:parameterized",
    ],
)

distribute_py_test(
    name = "custom_training_loop_optimizer_test",
    srcs = ["custom_training_loop_optimizer_test.py"],
    disable_mlir_bridge = False,
    main = "custom_training_loop_optimizer_test.py",
    tags = [
        "multi_and_single_gpu",
    ],
    deps = [
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:variables",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/distribute:values",
        "//tensorflow/python/eager:def_function",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/keras/optimizer_v2",
        "@absl_py//absl/testing:parameterized",
    ],
)

py_library(
    name = "distribute_strategy_test_lib",
    srcs = [
        "distribute_strategy_test.py",
    ],
    deps = [
        ":optimizer_combinations",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:training",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:distribute_lib",
        "//tensorflow/python/distribute:mirrored_strategy",
        "//tensorflow/python/distribute:parameter_server_strategy",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/distribute:tpu_strategy",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/estimator:estimator_py",
        "//tensorflow/python/keras",
        "//third_party/py/numpy",
        "@absl_py//absl/testing:parameterized",
    ],
)

distribute_py_test(
    name = "keras_premade_models_test",
    srcs = ["keras_premade_models_test.py"],
    disable_mlir_bridge = False,
    full_precision = True,
    main = "keras_premade_models_test.py",
    shard_count = 4,
    tags = [
        "multi_and_single_gpu",
    ],
    deps = [
        ":distribute_strategy_test_lib",
        ":keras_correctness_test_lib",
    ],
)

distribute_py_test(
    name = "distribute_strategy_test",
    srcs = ["distribute_strategy_test.py"],
    disable_mlir_bridge = True,  # TODO(b/170352626)
    full_precision = True,
    main = "distribute_strategy_test.py",
    python_version = "PY3",
    shard_count = 10,
    tags = [
        "multi_and_single_gpu",
        "no_rocm",  # times out on ROCm
        "no_windows_gpu",
        "noasan",  # TODO(b/170902997)
        "notap",  # TODO(b/170902997)
        "notsan",
    ],
    tpu_tags = [
        "no_oss",  # b/155502591
    ],
    deps = [
        ":distribute_strategy_test_lib",
        ":optimizer_combinations",
    ],
)

distribute_py_test(
    name = "distributed_training_utils_test",
    srcs = ["distributed_training_utils_test.py"],
    disable_mlir_bridge = False,
    full_precision = True,
    main = "distributed_training_utils_test.py",
    deps = [
        ":distribute",
        ":distribute_strategy_test_lib",
        "//tensorflow/python:platform",
        "//tensorflow/python:stateless_random_ops",
        "//tensorflow/python/keras:callbacks",
    ],
)

py_library(
    name = "keras_correctness_test_lib",
    srcs = [
        "keras_correctness_test_base.py",
        "keras_dnn_correctness_test.py",
        "keras_embedding_model_correctness_test.py",
        "keras_image_model_correctness_test.py",
        "keras_rnn_model_correctness_test.py",
        "keras_stateful_lstm_model_correctness_test.py",
    ],
    deps = [
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:training",
        "//tensorflow/python/distribute:collective_all_reduce_strategy",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:distribute_lib",
        "//tensorflow/python/distribute:mirrored_strategy",
        "//tensorflow/python/distribute:multi_process_runner",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/distribute:tpu_strategy",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/keras",
        "//tensorflow/python/keras:backend",
        "//third_party/py/numpy",
        "@absl_py//absl/testing:parameterized",
    ],
)

distribute_py_test(
    name = "keras_dnn_correctness_test",
    size = "medium",
    srcs = ["keras_dnn_correctness_test.py"],
    disable_mlir_bridge = True,  # TODO(b/170352626)
    full_precision = True,
    main = "keras_dnn_correctness_test.py",
    # Shard count is set to an odd number to distribute tasks across
    # shards more evenly.
    shard_count = 19,
    tags = [
        "multi_and_single_gpu",
        "no_rocm",  # times out on ROCm
        "no_windows_gpu",
        "nogpu",  # TODO(b/170905292)
        "notsan",
    ],
    deps = [
        ":keras_correctness_test_lib",
    ],
)

distribute_py_test(
    name = "keras_embedding_model_correctness_test",
    size = "medium",
    srcs = ["keras_embedding_model_correctness_test.py"],
    disable_mlir_bridge = True,  # TODO(b/170352626)
    full_precision = True,
    main = "keras_embedding_model_correctness_test.py",
    shard_count = 8,
    tags = [
        "multi_and_single_gpu",
        "no_windows_gpu",
        "notsan",
    ],
    deps = [
        ":keras_correctness_test_lib",
    ],
)

distribute_py_test(
    name = "keras_image_model_correctness_test",
    size = "medium",
    srcs = ["keras_image_model_correctness_test.py"],
    disable_mlir_bridge = True,  # TODO(b/170352626)
    full_precision = True,
    main = "keras_image_model_correctness_test.py",
    shard_count = 16,
    tags = [
        "multi_and_single_gpu",
        "no_rocm",  # times out on ROCm
        "no_windows_gpu",
        "notsan",
    ],
    xla_enable_strict_auto_jit = False,  # Tensorflow also fails.
    deps = [
        ":keras_correctness_test_lib",
    ],
)

distribute_py_test(
    name = "keras_metrics_test",
    srcs = ["keras_metrics_test.py"],
    disable_mlir_bridge = False,
    main = "keras_metrics_test.py",
    tags = [
        "multi_and_single_gpu",
    ],
    deps = [
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:math_ops",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/keras:metrics",
        "@absl_py//absl/testing:parameterized",
    ],
)

distribute_py_test(
    name = "keras_models_test",
    srcs = ["keras_models_test.py"],
    main = "keras_models_test.py",
    tags = [
        "multi_and_single_gpu",
    ],
    deps = [
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/keras",
        "@absl_py//absl/testing:parameterized",
    ],
)

distribute_py_test(
    name = "keras_rnn_model_correctness_test",
    size = "medium",
    srcs = ["keras_rnn_model_correctness_test.py"],
    full_precision = True,
    main = "keras_rnn_model_correctness_test.py",
    # Shard count is set to an odd number to distribute tasks across
    # shards more evenly.
    shard_count = 31,
    tags = [
        "multi_and_single_gpu",
        "no_cuda11",
        "no_oss",
        "no_windows_gpu",
        "notpu",  # TODO(b/153672562)
        "notsan",
    ],
    deps = [
        ":keras_correctness_test_lib",
    ],
)

distribute_py_test(
    name = "keras_save_load_test",
    size = "medium",
    srcs = ["keras_save_load_test.py"],
    full_precision = True,
    main = "keras_save_load_test.py",
    shard_count = 7,
    tags = [
        "multi_and_single_gpu",
    ],
    deps = [
        ":saved_model_test_base",
        "//tensorflow/python/keras/saving",
    ],
)

distribute_py_test(
    name = "keras_stateful_lstm_model_correctness_test",
    size = "medium",
    srcs = ["keras_stateful_lstm_model_correctness_test.py"],
    disable_mlir_bridge = False,
    full_precision = True,
    main = "keras_stateful_lstm_model_correctness_test.py",
    shard_count = 4,
    tags = [
        "multi_and_single_gpu",
        "no_pip",
        "no_windows_gpu",
        "notsan",
    ],
    deps = [
        ":keras_correctness_test_lib",
    ],
)

distribute_py_test(
    name = "keras_utils_test",
    srcs = ["keras_utils_test.py"],
    disable_mlir_bridge = True,  # TODO(b/170352626)
    full_precision = True,
    main = "keras_utils_test.py",
    shard_count = 4,
    tags = [
        "multi_and_single_gpu",
        "no_rocm",
        "no_windows_gpu",
        "notsan",
    ],
    deps = [
        ":keras_test_lib",
        ":optimizer_combinations",
        "//tensorflow/python/distribute:parameter_server_strategy",
        "//tensorflow/python/keras/distribute:distribute_strategy_test_lib",
    ],
)

py_library(
    name = "keras_test_lib",
    srcs = [
        "keras_utils_test.py",
    ],
    deps = [
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:training",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:mirrored_strategy",
        "//tensorflow/python/distribute:parameter_server_strategy",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/distribute:tpu_strategy",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/keras",
        "//third_party/py/numpy",
        "@absl_py//absl/testing:parameterized",
    ],
)

cuda_py_test(
    name = "keras_optimizer_v2_test",
    srcs = ["keras_optimizer_v2_test.py"],
    python_version = "PY3",
    shard_count = 4,
    tags = [
        "multi_and_single_gpu",
        "tf_integration_test",
    ],
    deps = [
        ":keras_test_lib",
    ],
)

distribute_py_test(
    name = "minimize_loss_test",
    srcs = ["minimize_loss_test.py"],
    main = "minimize_loss_test.py",
    tags = [
        "multi_and_single_gpu",
        "no_oss",  # TODO(b/139815303): enable after this is fixed.
        "noguitar",  # TODO(b/140755528): enable after this is fixed.
        "notap",  # TODO(b/139815303): enable after this is fixed.
    ],
    deps = [
        ":optimizer_combinations",
        "//tensorflow/python:control_flow_ops",
        "//tensorflow/python:control_flow_v2_toggles",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:math_ops",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python:variables",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:mirrored_strategy",
        "//tensorflow/python/distribute:single_loss_example",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/distribute:strategy_test_lib",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/ops/losses",
        "//third_party/py/numpy",
        "@absl_py//absl/testing:parameterized",
    ],
)

cuda_py_test(
    name = "mirrored_strategy_test",
    srcs = ["mirrored_strategy_test.py"],
    python_version = "PY3",
    tags = [
        "multi_and_single_gpu",
        "no_windows_gpu",  # TODO(b/130551176)
    ],
    deps = [
        "//tensorflow/python:array_ops",
        "//tensorflow/python:training_lib",
        "//tensorflow/python:variables",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/eager:backprop",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:function",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/keras/engine",
        "//tensorflow/python/keras/layers:core",
    ],
)

cuda_py_test(
    name = "mirrored_variable_test",
    srcs = ["mirrored_variable_test.py"],
    python_version = "PY3",
    tags = [
        "guitar",
        "multi_and_single_gpu",
    ],
    deps = [
        "//tensorflow/python:config",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/distribute:collective_all_reduce_strategy",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:distribute_lib",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/distribute:values",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/keras:metrics",
        "//tensorflow/python/keras/layers:core",
    ],
)

cuda_py_test(
    name = "multi_worker_test",
    srcs = ["multi_worker_test.py"],
    python_version = "PY3",
    shard_count = 2,
    tags = [
        "multi_and_single_gpu",
        "no_oss",  # TODO(b/130369494): Investigate why it times out on OSS.
    ],
    deps = [
        ":multi_worker_testing_utils",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:dtypes",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:random_ops",
        "//tensorflow/python:util",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/distribute:collective_all_reduce_strategy",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:distribute_coordinator",
        "//tensorflow/python/distribute:distribute_coordinator_context",
        "//tensorflow/python/distribute:distribute_lib",
        "//tensorflow/python/distribute:multi_worker_test_base",
        "//tensorflow/python/distribute:parameter_server_strategy",
        "//tensorflow/python/distribute/cluster_resolver:cluster_resolver_lib",
        "//tensorflow/python/keras",
        "//tensorflow/python/keras:backend",
        "//tensorflow/python/keras:callbacks",
        "//tensorflow/python/keras:engine",
        "//tensorflow/python/keras:optimizers",
        "//tensorflow/python/keras/optimizer_v2",
        "@absl_py//absl/testing:parameterized",
    ],
)

py_test(
    name = "multi_worker_callback_tf2_test",
    srcs = ["multi_worker_callback_tf2_test.py"],
    python_version = "PY3",
    shard_count = 5,
    deps = [
        "//tensorflow/python/distribute:collective_all_reduce_strategy",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:multi_process_runner",
        "//tensorflow/python/distribute:multi_worker_test_base",
        "//tensorflow/python/keras/distribute:multi_worker_testing_utils",
    ],
)

py_library(
    name = "multi_worker_testing_utils",
    srcs = [
        "multi_worker_testing_utils.py",
    ],
    deps = [
        "//tensorflow/python:array_ops",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:random_ops",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/keras",
        "//tensorflow/python/keras/optimizer_v2",
    ],
)

py_library(
    name = "tpu_strategy_test_utils",
    srcs = ["tpu_strategy_test_utils.py"],
    srcs_version = "PY2AND3",
    deps = [
        "//tensorflow/python:platform",
        "//tensorflow/python/distribute:tpu_strategy",
        "//tensorflow/python/distribute/cluster_resolver:tpu_cluster_resolver_py",
        "//tensorflow/python/eager:remote",
        "//tensorflow/python/tpu:tpu_strategy_util",
    ],
)

py_test(
    name = "multi_worker_tutorial_test",
    srcs = ["multi_worker_tutorial_test.py"],
    python_version = "PY3",
    shard_count = 5,
    tags = [
        "noasan",  # TODO(b/156029134)
        "nomsan",  # TODO(b/156029134)
        "notap",  # TODO(b/165865820): restore when not flaky
        "notsan",  # TODO(b/156029134)
    ],
    deps = [
        "//tensorflow/python:platform",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/distribute:collective_all_reduce_strategy",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:multi_process_runner",
        "//tensorflow/python/distribute:multi_worker_test_base",
        "//tensorflow/python/keras",
        "//tensorflow/python/keras/optimizer_v2",
    ],
)

distribute_py_test(
    name = "saved_model_save_load_test",
    size = "medium",
    srcs = ["saved_model_save_load_test.py"],
    full_precision = True,
    main = "saved_model_save_load_test.py",
    shard_count = 7,
    tags = [
        "multi_and_single_gpu",
        "no_rocm",
    ],
    deps = [
        ":saved_model_test_base",
        "//tensorflow/python/saved_model",
    ],
)

distribute_py_test(
    name = "saved_model_mixed_api_test",
    size = "medium",
    srcs = ["saved_model_mixed_api_test.py"],
    full_precision = True,
    main = "saved_model_mixed_api_test.py",
    shard_count = 7,
    tags = [
        "multi_and_single_gpu",
        "no_rocm",
    ],
    deps = [
        ":saved_model_test_base",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/keras/saving",
    ],
)

distribute_py_test(
    name = "step_fn_test",
    srcs = ["step_fn_test.py"],
    main = "step_fn_test.py",
    tags = [
        "multi_and_single_gpu",
    ],
    deps = [
        ":optimizer_combinations",
        "//tensorflow/python:framework_test_lib",
        "//tensorflow/python:variables",
        "//tensorflow/python/distribute:combinations",
        "//tensorflow/python/distribute:single_loss_example",
        "//tensorflow/python/distribute:strategy_combinations",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:test",
        "//third_party/py/numpy",
        "@absl_py//absl/testing:parameterized",
    ],
)

py_test(
    name = "parameter_server_training_test",
    srcs = ["parameter_server_training_test.py"],
    python_version = "PY3",
    shard_count = 1,
    tags = [
        "no_oss",  # TODO(b/162119374): enable it in OSS.
    ],
    deps = [
        "//tensorflow/python:constant_op",
        "//tensorflow/python:dtypes",
        "//tensorflow/python:init_ops_v2",
        "//tensorflow/python:training_server_lib",
        "//tensorflow/python:variables",
        "//tensorflow/python/compat:v2_compat",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/distribute:multi_worker_test_base",
        "//tensorflow/python/distribute:parameter_server_strategy_v2",
        "//tensorflow/python/distribute:sharded_variable",
        "//tensorflow/python/distribute/client",
        "//tensorflow/python/distribute/cluster_resolver:cluster_resolver_lib",
        "//tensorflow/python/eager:backprop",
        "//tensorflow/python/eager:def_function",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/keras",
    ],
)
