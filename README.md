# P[0] = 1
addi $t0, $zero, 1
sw   $t0, 0($s2)

# k = 1
addi $s3, $zero, 1

# j = 1
addi $t0, $zero, 1

loop:
# Stop when j >= n
slt  $t1, $t0, $s1
beq  $t1, $zero, done

# Get A[j]
sll  $t2, $t0, 2
add  $t2, $s0, $t2
lw   $a0, 0($t2)

# Call power(A[j], j)
add  $a1, $t0, $zero
jal  power

# Call newElement(P, k, pow)
add  $a0, $s2, $zero
add  $a1, $s3, $zero
add  $a2, $v0, $zero
jal  newElement

# k++
addi $s3, $s3, 1

# j++
addi $t0, $t0, 1

j loop


# power(a, b)
# Returns a^b in $v0
power:
add  $v0, $a0, $zero
addi $t3, $zero, 1

power_loop:
slt  $t4, $t3, $a1
beq  $t4, $zero, power_done

mult $v0, $a0
mflo $v0

addi $t3, $t3, 1
j power_loop

power_done:
jr $ra


# newElement(P, k, pow)
# Stores pow into P[k]
newElement:
sll  $t3, $a1, 2
add  $t3, $a0, $t3
sw   $a2, 0($t3)

jr $ra


done:
