---
title: test
date: 2022-10-28 19:04:17
tags:
---

# Hello South
``` c++
/***************************************************************************************************
 * author: hexueyuan
 * create: Thu 14 Apr 2022 03:06:50 PM CST
 * file  : babylon_string_split_benchmark.cpp
 * description:
 * Copyright (c) 2021 Baidu.com, Inc. All Rights Reserved
***************************************************************************************************/

#include <string>

#include <benchmark/benchmark.h>
#include <baidu/feed/mlarch/babylon/lite/string.h>

static void split_by_char(benchmark::State& state) {
    size_t split_cnt = state.range(0);

    const static char delim = '|';
    const static std::string part("0123456789");

    std::string string;
    string.reserve((part.size() + 1) * split_cnt);

    for (size_t i = 0; i < split_cnt; ++i) {
        string.append(part);
        if (i != split_cnt - 1) {
            string.append(std::string(1, delim));
        }
    }

    std::vector<std::string> parts(split_cnt);
    for (auto _ : state) {
        baidu::feed::mlarch::babylon::split(parts, string, delim);
    }
}

static void split_by_string(benchmark::State& state) {
    size_t split_cnt = state.range(0);

    const static std::string delim = "|";
    const static std::string part("0123456789");

    std::string string;
    string.reserve((part.size() + 1) * split_cnt);

    for (size_t i = 0; i < split_cnt; ++i) {
        string.append(part);
        if (i != split_cnt - 1) {
            string.append(delim);
        }
    }

    std::vector<std::string> parts(split_cnt);
    for (auto _ : state) {
        baidu::feed::mlarch::babylon::split(parts, string, delim);
    }
}

BENCHMARK(split_by_char)->Arg(1)->Arg(8)->Arg(32)->Arg(128)->Arg(1024);
BENCHMARK(split_by_string)->Arg(1)->Arg(8)->Arg(32)->Arg(128)->Arg(1024);
BENCHMARK_MAIN();
```
