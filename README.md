# P[0] = 1
addi $t0, $zero, 1
sw   $t0, 0($s2)

# P currently contains one element
addi $s3, $zero, 1

# i = 1
addi $t1, $zero, 1


loop:
    # Check if i < n
    slt  $t2, $t1, $s1
    beq  $t2, $zero, done

    # Get address of A[i]
    sll  $t3, $t1, 2
    add  $t4, $s0, $t3

    # Load A[i]
    lw   $a0, 0($t4)

    # Second argument is index i
    add  $a1, $t1, $zero

    # Find A[i]^i
    jal  power

    # Arguments for newElement
    add  $a0, $s2, $zero
    add  $a1, $s3, $zero
    add  $a2, $v0, $zero

    # Store result in P
    jal  newElement

    # k++
    addi $s3, $s3, 1

    # i++
    addi $t1, $t1, 1

    j loop


power:
    # result = 1
    addi $v0, $zero, 1

    # counter = 0
    add  $t5, $zero, $zero

powerLoop:
    # Check counter < index
    slt  $t6, $t5, $a1
    beq  $t6, $zero, powerDone

    # result = result * element
    mult $v0, $a0
    mflo $v0

    # counter++
    addi $t5, $t5, 1

    j powerLoop

powerDone:
    jr $ra


newElement:
    # Offset = k * 4
    sll  $t7, $a1, 2

    # Address of P[k]
    add  $t8, $a0, $t7

    # P[k] = new element
    sw   $a2, 0($t8)

    jr $ra


done:
