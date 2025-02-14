# python/lib/core package

load("//tensorflow:tensorflow.default.bzl", "get_compatible_with_portable", "tf_external_workspace_visible", "tf_py_test", "tf_python_pybind_extension")

visibility = [
    "//engedu/ml/tf_from_scratch:__pkg__",
    "//third_party/cloud_tpu/convergence_tools:__subpackages__",
    "//third_party/mlperf:__subpackages__",
    "//tensorflow:internal",
    "//tensorflow/lite/toco/python:__pkg__",
    "//tensorflow_models:__subpackages__",
    "//tensorflow_model_optimization:__subpackages__",
    "//third_party/py/cleverhans:__subpackages__",
    "//third_party/py/launchpad:__subpackages__",
    "//third_party/py/reverb:__subpackages__",
    "//third_party/py/neural_structured_learning:__subpackages__",
    "//third_party/py/tensorflow_examples:__subpackages__",
    "//third_party/py/tf_agents:__subpackages__",  # For benchmarks.
    "//third_party/py/tf_slim:__subpackages__",
    "//third_party/py/tensorflow_docs:__subpackages__",
    "//third_party/py/keras:__subpackages__",
]

package(
    # copybara:uncomment default_applicable_licenses = ["//tensorflow:license"],
    default_visibility = visibility,
    licenses = ["notice"],
)

cc_library(
    name = "custom_float",
    hdrs = [
        "custom_float.h",
    ],
    deps = [
        "//tensorflow/core/platform:logging",
        "//tensorflow/core/platform:types",
        "//tensorflow/tsl/python/lib/core:numpy",
        "//third_party/eigen3",
        "//third_party/python_runtime:headers",  # build_cleaner: keep; DNR: b/35864863
        "@com_google_absl//absl/strings",
    ],
)

cc_library(
    name = "bfloat16_lib",
    srcs = [
        "bfloat16.cc",
        "float8_e4m3b11.cc",
    ],
    hdrs = [
        "bfloat16.h",
        "float8_e4m3b11.h",
    ],
    deps = [
        ":custom_float",
        "//tensorflow/core/platform:logging",
        "//tensorflow/core/platform:types",
        "//tensorflow/tsl/python/lib/core:numpy",
        "//third_party/eigen3",
        "//third_party/python_runtime:headers",  # build_cleaner: keep; DNR: b/35864863
        "@com_google_absl//absl/strings",
    ],
)

tf_python_pybind_extension(
    name = "_pywrap_bfloat16",
    srcs = ["bfloat16_wrapper.cc"],
    hdrs = ["bfloat16.h"],
    deps = [
        "//third_party/python_runtime:headers",
        "@pybind11",
    ],
)

cc_library(
    name = "float8_lib",
    srcs = [
        "float8.cc",
    ],
    hdrs = [
        "float8.h",
    ],
    deps = [
        ":custom_float",
        "//tensorflow/core/platform:float8",
        "//tensorflow/core/platform:logging",
        "//tensorflow/core/platform:types",
        "//tensorflow/tsl/python/lib/core:numpy",
        "//third_party/eigen3",
        "//third_party/python_runtime:headers",  # build_cleaner: keep; DNR: b/35864863
        "@com_google_absl//absl/strings",
    ],
)

tf_python_pybind_extension(
    name = "_pywrap_float8",
    srcs = ["float8_wrapper.cc"],
    hdrs = ["float8.h"],
    deps = [
        ":float8_lib",
        "//third_party/python_runtime:headers",
        "@pybind11",
    ],
)

cc_library(
    name = "custom_types_lib",
    deps = [
        ":bfloat16_lib",
        ":float8_lib",
    ],
)

cc_library(
    name = "ndarray_tensor_bridge",
    srcs = ["ndarray_tensor_bridge.cc"],
    hdrs = ["ndarray_tensor_bridge.h"],
    visibility = tf_external_workspace_visible(
        visibility + [
            "//tensorflow:ndarray_tensor_allow_list",
        ],
    ),
    deps = [
        ":bfloat16_lib",
        ":float8_lib",
        ":py_util",
        "//tensorflow/c:c_api_no_xla",
        "//tensorflow/core:lib",
        "//tensorflow/core:protos_all_cc",
        "//tensorflow/tsl/python/lib/core:numpy",
    ],
)

cc_library(
    name = "py_exception_registry",
    srcs = ["py_exception_registry.cc"],
    hdrs = ["py_exception_registry.h"],
    deps = [
        "//tensorflow/c:tf_status_headers",
        "//tensorflow/core:lib",
        "//tensorflow/core:protos_all_cc",
        "//third_party/python_runtime:headers",
    ],
    alwayslink = 1,
)

cc_library(
    name = "pybind11_absl",
    hdrs = ["pybind11_absl.h"],
    features = ["-parse_headers"],
    visibility = tf_external_workspace_visible(visibility),
    deps = [
        "//tensorflow/core/platform:stringpiece",
        "@pybind11",
    ],
)

cc_library(
    name = "pybind11_lib",
    hdrs = ["pybind11_lib.h"],
    compatible_with = get_compatible_with_portable(),
    features = ["-parse_headers"],
    visibility = tf_external_workspace_visible(visibility),
    deps = [
        "@pybind11",
    ],
)

cc_library(
    name = "pybind11_status_headers",
    hdrs = [
        "py_exception_registry.h",
        "pybind11_status.h",
        "//tensorflow/c:headers",
        "//tensorflow/c:tf_status_internal_headers",
        "//tensorflow/c/eager:headers",
    ],
    features = [
        "-parse_headers",
    ],
    visibility = tf_external_workspace_visible(visibility),
    deps = [
        "//tensorflow/core:protos_all_cc",
        "//tensorflow/core/common_runtime:core_cpu_headers_lib",
        "//third_party/python_runtime:headers",
        "@pybind11",
    ],
)

cc_library(
    name = "pybind11_status",
    hdrs = [
        "py_exception_registry.h",
        "pybind11_status.h",
    ],
    features = ["-parse_headers"],
    visibility = tf_external_workspace_visible(visibility),
    deps = [
        ":pybind11_status_headers",
        "//tensorflow/core:lib",
    ],
)

cc_library(
    name = "pybind11_proto",
    hdrs = ["pybind11_proto.h"],
    features = ["-parse_headers"],
    visibility = tf_external_workspace_visible(visibility),
    deps = [
        "@com_google_absl//absl/strings",
        "@pybind11",
    ],
)

filegroup(
    name = "py_exception_registry_hdr",
    srcs = [
        "py_exception_registry.h",
    ],
    visibility = ["//visibility:public"],
)

filegroup(
    name = "safe_ptr_hdr",
    srcs = ["safe_ptr.h"],
)

filegroup(
    name = "ndarray_tensor_hdr",
    srcs = ["ndarray_tensor.h"],
)

filegroup(
    name = "basic_hdrs",
    srcs = [
        "bfloat16.h",
        "float8.h",
        "ndarray_tensor.h",
        "ndarray_tensor_bridge.h",
        "py_exception_registry.h",
        "pybind11_status.h",
        "safe_ptr.h",
        "safe_pyobject_ptr.h",
    ],
)

cc_library(
    name = "py_func_lib",
    srcs = ["py_func.cc"],
    hdrs = ["py_func.h"],
    deps = [
        ":ndarray_tensor",
        ":ndarray_tensor_bridge",
        ":py_util",
        ":safe_ptr",
        "//tensorflow/c:tf_status_helper",
        "//tensorflow/c/eager:c_api",
        "//tensorflow/c/eager:tfe_context_internal",
        "//tensorflow/c/eager:tfe_tensorhandle_internal",
        "//tensorflow/core:framework",
        "//tensorflow/core:lib",
        "//tensorflow/core:protos_all_cc",
        "//tensorflow/core:script_ops_op_lib",
        "//tensorflow/core/common_runtime/eager:context",
        "//tensorflow/core/common_runtime/eager:tensor_handle",
        "//tensorflow/python/eager:pywrap_tfe_lib",
        "//tensorflow/tsl/python/lib/core:numpy",
        "//third_party/py/numpy:headers",
        "//third_party/python_runtime:headers",
    ],
    alwayslink = 1,
)

tf_python_pybind_extension(
    name = "_pywrap_py_func",
    srcs = ["py_func_wrapper.cc"],
    deps = [
        "//tensorflow/python:py_func_headers_lib",
        "//third_party/python_runtime:headers",
        "@pybind11",
    ],
)

cc_library(
    name = "safe_pyobject_ptr",
    srcs = ["safe_pyobject_ptr.cc"],
    hdrs = ["safe_pyobject_ptr.h"],
    deps = [
        "//third_party/python_runtime:headers",
    ],
)

cc_library(
    name = "safe_pyobject_ptr_required_hdrs",
    textual_hdrs = ["safe_pyobject_ptr.h"],
)

cc_library(
    name = "safe_ptr",
    srcs = [
        "safe_ptr.cc",
        "//tensorflow/c/eager:headers",
    ],
    hdrs = ["safe_ptr.h"],
    deps = [
        ":safe_pyobject_ptr",
        "//tensorflow/c:c_api_no_xla",
        "//third_party/python_runtime:headers",
    ],
)

cc_library(
    name = "ndarray_tensor_headers",
    hdrs = [
        "bfloat16.h",
        "float8.h",
        "ndarray_tensor.h",
        "ndarray_tensor_bridge.h",
        "safe_ptr.h",
        "safe_pyobject_ptr.h",
        "//tensorflow/c:headers",
        "//tensorflow/c/eager:headers",
        "//tensorflow/tsl/python/lib/core:numpy_hdr",
    ],
    features = [
        "-parse_headers",
    ],
    visibility = tf_external_workspace_visible(visibility + [
        "//tensorflow:ndarray_tensor_allow_list",
    ]),
    deps = [
        "//tensorflow/c:pywrap_required_hdrs",
        "//tensorflow/c:tf_status_headers",
        "//tensorflow/core:framework_internal_headers_lib",
        "//tensorflow/core/common_runtime:core_cpu_headers_lib",
        "//tensorflow/tsl/python/lib/core:numpy",
        "//third_party/py/numpy:headers",
        "//third_party/python_runtime:headers",
    ],
)

cc_library(
    name = "ndarray_tensor",
    srcs = ["ndarray_tensor.cc"],
    hdrs = ["ndarray_tensor.h"],
    visibility = tf_external_workspace_visible(visibility + [
        "//tensorflow:ndarray_tensor_allow_list",
    ]),
    deps = [
        ":bfloat16_lib",
        ":float8_lib",
        ":ndarray_tensor_bridge",
        ":safe_ptr",
        "//tensorflow/c:c_api_internal",
        "//tensorflow/c:tf_status_helper",
        "//tensorflow/c:tf_tensor_internal",
        "//tensorflow/c/eager:tfe_context_internal",
        "//tensorflow/core:framework",
        "//tensorflow/core:lib",
        "//tensorflow/tsl/python/lib/core:numpy",
    ],
)

cc_library(
    name = "py_seq_tensor",
    srcs = ["py_seq_tensor.cc"],
    hdrs = ["py_seq_tensor.h"],
    features = ["-parse_headers"],
    deps = [
        ":ndarray_tensor",
        ":ndarray_tensor_bridge",
        ":py_util",
        ":safe_ptr",
        "//tensorflow/c:tensor_interface",
        "//tensorflow/c:tf_tensor_internal",
        "//tensorflow/c/eager:c_api_internal",
        "//tensorflow/c/eager:tfe_context_internal",
        "//tensorflow/c/eager:tfe_tensorhandle_internal",
        "//tensorflow/core:framework",
        "//tensorflow/core:lib",
        "//tensorflow/tsl/python/lib/core:numpy",
        "//third_party/python_runtime:headers",  # build_cleaner: keep; DNR: b/35864863
    ],
)

cc_library(
    name = "py_util",
    srcs = ["py_util.cc"],
    hdrs = ["py_util.h"],
    deps = [
        "//tensorflow/core:lib",
        "//tensorflow/core:script_ops_op_lib",
        "//tensorflow/core/platform:logging",
        "//third_party/python_runtime:headers",
    ],
)

tf_py_test(
    name = "custom_float_test",
    size = "small",
    srcs = ["custom_float_test.py"],
    python_version = "PY3",
    deps = [
        "//tensorflow/python:pywrap_tensorflow",
        "//tensorflow/python/lib/io:lib",
        "//tensorflow/python/platform:client_testlib",
    ],
)
