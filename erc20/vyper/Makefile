BUILD_DIR:=../../.build

LOCAL_LEMMAS:=../verification.k \
			  ../../resources/evm-symbolic.k ../../resources/evm-data-symbolic.k \
			  ../../resources/abstract-semantics-segmented-gas.k
TMPLS:=../module-tmpl.k ../spec-tmpl.k

SPEC_GROUP:=vyper-erc20
SPEC_INI:=vyper-erc20-spec.ini

KPROVE_OPTS:=--smt-prelude $(abspath $(dir $(abspath $(lastword $(MAKEFILE_LIST))))/../../resources/evm.smt2)

SPEC_NAMES:=totalSupply \
            balanceOf \
            allowance \
            approve \
            transfer-success-1 \
            transfer-success-2 \
            transfer-failure-1 \
            transfer-failure-2 \
            transferFrom-success-1 \
            transferFrom-success-2 \
            transferFrom-failure-1 \
            transferFrom-failure-2

include ../../resources/kprove.mak
